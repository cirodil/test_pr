<template>
  <div>
    <el-menu
      :default-active="$route.path"
      class="el-menu-demo"
      mode="horizontal"
      router
    >
      <el-menu-item index="/">Главная</el-menu-item>
      <el-menu-item index="/login">Авторизация</el-menu-item>
      <el-menu-item index="/shop">Магазин</el-menu-item>
    </el-menu>
    <div class="line"></div>
  </div>
</template>

<script>
export default {
  name: 'Navigation',
}
</script>

<style></style>
