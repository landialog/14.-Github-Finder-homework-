<template>
  <div class="wrapper-content wrapper-content--fixed">
    <section>
      <div class="container">
        <!-- search -->
        <search
          @unSubmit="getRepos"
          :value="search"
          placeholder="Enter users"
          @search="search = $event"
        />
        <!-- buttons -->
        <button @click="getRepos" class="btn btnPrimary">
          <span v-if="!repos">Search!</span>
          <span v-else>Search Again</span>
        </button>

        <!-- user block -->
        <user v-if="user" :user="user" />

        <!-- wrapper -->
        <div class="repos_wrapper" v-if="repos">
          <table>
            <!--              head-->
            <thead>
              <tr>
                <th @click="sort('name')">Name</th>
                <th @click="sort('star')">Star</th>
              </tr>
            </thead>

            <tbody>
              <tr v-for="item in repoSort" :key="item.id">
                <td><a class="link" target="_blank" :href="item.html_url">{{item.name}}</a></td>
                <td>{{item.stargazers_count}}⭐</td>
              </tr>
            </tbody>
          </table>
          <div class="section">
              <div class="button-list">
                <p>{{page.current}}</p>
                <div class="btn btnPrimary" @click="prevPage">&#8592;</div>
                <div class="btn btnPrimary" @click="nextPage">&#8594;</div>
              </div>
          </div>
        </div>
        <!--  error -->
        <h3 v-if="error" class="error">Nothing found</h3>
      </div>
    </section>
  </div>
</template>

<script>
import search from "@/components/Search";
import axios from "axios";
import user from "@/components/User";

export default {
  components: { search, user },
  data() {
    return {
      search: "vedees",
      repos: null,
      error: null,
      user: "",
      currentSort: "name",
      currentSortDir: "asc",
      page: {
        current: 1,
        length: 3
      }
    };
  },

  computed: {
    repoSort() {
      return this.repos.sort((a, b) => {
          let mod = 1;
          if (this.currentSortDir === "desc") mod = -1
          if (a[this.currentSort] < b[this.currentSort]) return -1 * mod
          if (a[this.currentSort] > b[this.currentSort]) return 1 * mod
          return 0;
        })
        .filter((row, index) => {
          let start = (this.page.current - 1) * this.page.length;
          let end = this.page.current * this.page.length;
          if (index >= start && index < end) return true;
        });
    }
  },

  methods: {
    getRepos() {
      axios
        .all([
          axios.get(`https://api.github.com/users/${this.search}`),
          axios.get(`https://api.github.com/users/${this.search}/repos`)
        ])
        .then(res => {
          this.user = res[0].data;
          this.repos = res[1].data;
          this.error = null;
          // console.log(res)
        })
        .catch(err => {
          //console.log(err)
          this.repos = null;
          this.error = "Nobody";
        });
      //console.log(`Find ${this.search}`)
    },

    sort(e) {
      if (e === this.currentSort) {
        this.currentSortDir = this.currentSortDir === "asc" ? "desc" : "asc";
      }
      this.currentSort = e;
    },
    prevPage() {
      if (this.page.current > 1) this.page.current -= 1;
    },
    nextPage() {
      if (this.page.current * this.page.length < this.repos.length)
        this.page.current += 1;
    }
  }
};
</script>

<style lang="scss">
img {
  margin-right: 16px;
  border-radius: 50%;
}

.button-list {
  width: 100%;
  text-align: center;
}

.container {
  display: flex;
  align-items: center;
  flex-direction: column;
}

.btn {
  margin-top: 40px;
}

.repos_wrapper {
  width: 400px;
  margin-top: 20px;
}

.repos-info {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 10px;
  padding: 5px 0;
  border-bottom: 1px solid #141ddb;
}

.error {
  margin-top: 10px;
  color: #cc0000;
}

.user_item {
  margin-top: 20px;
  width: 400px;
}

.user_rigth,
.user_left {
  width: 200px;

  display: inline-block;
}

.user_info {
  display: block;
}
</style>
