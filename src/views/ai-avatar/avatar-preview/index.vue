<script lang="ts" setup>
import {} from 'vue';
import Menu from './modules/menu.vue';
import Avatar from './modules/avatar.vue';
</script>

<template>
  <div class="wrapper">
    <Menu></Menu>
    <div class="avatar-wrapper">
      <Avatar></Avatar>
    </div>
  </div>
</template>

<style scoped lang="scss">
.wrapper {
  display: flex;
}
.avatar-wrapper {
  flex: 1;
  background-color: #fff;
  height: 100%;
  margin-left: 15px;
  padding: 10px;
  border-radius: 10px;
}
</style>
