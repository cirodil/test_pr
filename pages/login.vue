<template>
  <div id="login">
    <Login />
  </div>
</template>

<script>
import Login from '../components/Login.vue'

export default {
  components: { Login },
}
</script>

<style lang="scss">
#login {
  font-family: Roboto, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 0;
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}
body {
  margin: 0;
  padding: 0;
  background: #e9ebec;
  background-size: contain;
}
</style>
