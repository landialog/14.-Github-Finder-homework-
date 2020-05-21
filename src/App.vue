<template>
  <div class="wrapper">
    <header>
      <div class="navbar">
        <div class="container">
          <div class="navbar-content">
            <div class="logo">vue-github</div>
                               <ul class="navbar-list">
                                   <li v-for="item in links" class="navbar-item" :key="item.title">
            <router-link :title="item.title" :to="item.url" class="navbar-link" >{{item.title}}</router-link>
            </li>
            </ul>
          </div>
        </div>
      </div>
    </header>
    <router-view></router-view>
  </div>
</template>

<script>
export default {
  data() {
    return {
      links: [{ title: "Home", url: "/" }]
    };
  }
};
</script>

<style lang="scss">
.navbar-link {
  &.router-link-exact-active {
    color: #1517cc;
  }
}

#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.wrapper {
  background-color: #e7e7e7;
}

.normal {
  background-color: #23cc32;
}

.warning {
  background-color: yellow;
}

.danger {
  background-color: #ff2b3e;
  color: #ffffff;
}
</style>
