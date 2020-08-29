<template>
  <div id="app">
    <Header />
    <router-view />
    <footer>
      <p>Created by Jovana Stojanovic, 2020</p>
    </footer>
  </div>
</template>
<script>
import AddComment from "./components/AddComment.vue";
import Header from "./components/Header.vue";
export default {
  components: {
    AddComment,
    Header,
  },
  data() {
    return {
      modalActive: true,
    };
  },
};
</script>

<style>
html,
body {
  padding: 0;
  margin: 0;
  font-family: "Open Sans", Arial, sans-serif;
  font-size: 14px;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
}

footer {
  padding: 15px;
  background-color: rgba(56, 21, 13, 0.534);
}
footer p {
  margin: 0;
  text-align: center;
  color: white;
}
</style>
