<template>
  <div id="app">
    <Header @open-modal="changeWhichForm" :loggedInUserId="loggedInUserId" />
    <div class="quote">
      <p>Prodaja kopija poznatih umetnickih dela</p>
    </div>
    <Modal
      :whichForm="whichForm"
      @close-modal="clearWhichForm"
      @open-login="openLogin"
      @success="loginUser"
      @logout="logoutUser"
    />
    <router-view :loggedInUserId="loggedInUserId" />
    <footer>
      <p>Created by Jovana Stojanovic, 2020</p>
    </footer>
  </div>
</template>
<script>
import Header from "./components/Header.vue";
import Modal from "./components/Modal.vue";
export default {
  components: {
    Header,
    Modal,
  },
  data() {
    return {
      whichForm: "",
      loggedInUserId: localStorage.getItem("user_id"),
    };
  },
  methods: {
    changeWhichForm(whichForm) {
      this.whichForm = whichForm;
    },
    clearWhichForm() {
      this.whichForm = "";
    },
    loginUser(user_id) {
      this.loggedInUserId = user_id;
    },
    logoutUser() {
      this.loggedInUserId = "";
    },
    openLogin() {
      this.whichForm = "login";
    },
  },
};
</script>

<style>
html,
body,
p,
h1,
h2,
h3 {
  padding: 0;
  margin: 0;
  font-family: "Open Sans", Arial, Helvetica, sans-serif;
}
#app {
  /* font-family: "Open Sans", Arial, Helvetica, sans-serif; */
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
}
.quote {
  background-color: rgb(23, 112, 112);
  padding-bottom: 5px;
}
.quote p {
  color: rgb(255, 255, 255);
  font-size: 15px;
  font-family: cursive;
  font-style: italic;
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
