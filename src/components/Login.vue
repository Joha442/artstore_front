<template>
  <form @submit.prevent="login">
    <h1>
      <i class="fas fa-user"></i>
    </h1>
    <input type="text" v-model="username" placeholder="Username" class="forms" />
    <input type="password" v-model="password" placeholder="Password" class="forms" />
    <button type="submit" class="button">PRIJAVA</button>
  </form>
</template>

<script>
import axios from "axios";
export default {
  data: function () {
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
          this.$emit("success", "user_id");
          this.$emit("close-modal");
        })
        .catch((err) => {
          console.log(err.message);
        });
    },
  },
};
</script>

<style>
h1 {
  color: rgba(56, 21, 13, 0.534);
  font-size: 50px;
}
.forms {
  font-family: FontAwesome, Arial, Helvetica, sans-serif;
  display: block;
  margin: 20px;
  padding: 5px;
  width: 250px;
  height: 25px;
  background-color: rgba(56, 21, 13, 0.274);
  border: rgba(56, 21, 13, 0.068) 1px solid;
}
.button {
  width: 263px;
  height: 35px;
  margin-bottom: 40px;
  color: rgb(255, 255, 255);
  background-color: rgba(56, 21, 13, 0.534);
  padding: 5px;
  border: none;
  box-shadow: 3px 3px 3px 0px rgba(0, 0, 0, 0.699);
}
.button:hover {
  background-color: rgb(23, 112, 112);
}
</style>
