<template>
  <form @submit.prevent="login">
    <input type="text" v-model="username" placeholder="Username" class="forms" />
    <input type="password" v-model="password" placeholder="Password" class="forms" />
    <button type="submit" class="forms button">Login</button>
  </form>
</template>

<script>
import axios from "axios";
export default {
  data: function() {
    return {
      username: "",
      password: "",
    };
  },
  methods: {
    login() {
      axios
        .post("http://localhost:3000/login", {
          username: this.username.trim(),
          password: this.password,
        })
        .then((res) => {
          console.log(res.data);
          localStorage.setItem("user_id", res.data.data.user_id);
        })
        .catch((err) => {
          console.log(err.message);
        });
    },
  },
};
</script>

<style>
.forms {
  display: block;
  margin: 10px;
  padding: 5px;
  width: 250px;
  height: 25px;
  background-color: rgba(56, 21, 13, 0.274);
  /* background-color: rgb(172, 226, 172); */
  border: rgba(56, 21, 13, 0.068) 1px solid;
}
.button {
  width: 263px;
}
</style>
