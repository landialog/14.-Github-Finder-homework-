<template>
  <div class="user_item">
    <div class="user_left">
      <img :src="user.avatar_url" alt="user.name" height="150" width="150" />
    </div>
    <div class="user_right">
      <div class="user_info">
        <h3>Имя: {{user.name}}</h3>
      </div>
    </div>
    <div class="user_info">
      <p>Количество репозиториев: {{user.public_repos}}</p>
    </div>
  </div>
</template>

<script>
export default {
  props:{
    user:{
      type: Object,
    }
  }
};
</script>

<style>
</style>