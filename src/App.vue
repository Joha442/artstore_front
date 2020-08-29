<template>
  <div id="app">
    <header>
      <div class="logo">
        <img class="logo" src="./assets/logo.png" alt="logo-slika" />
      </div>
      <!--end logo-->
      <div id="nav">
        <router-link to="/">Home</router-link> | <router-link to="/login">login</router-link> |
        <router-link to="/register">register</router-link> | <router-link to="/order">Order</router-link> |
        <router-link to="/ShowMore">Show More</router-link>
      </div>
    </header>
    <router-view />
  </div>
</template>
<script>
import AddComment from "./components/AddComment.vue";
export default {
  components: {
    AddComment,
  },
};
</script>

<style>
html,
body {
  padding: 0;
  margin: 0;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  text-align: center;
}
header {
  background-color: rgba(119, 26, 65, 0.575);
  box-shadow: 5px 11px 11px -8px rgba(0, 0, 0, 0.8);
  display: flex;
  justify-content: space-between;
  align-items: center;
  align-content: center;
}

#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}
.logo img {
  max-width: 200px;
}
.logo {
  margin-left: 5px;
}
</style>
