<template>
  <form @submit.prevent="login">
    <h1>
      <i class="fas fa-user"></i>
    </h1>
    <input type="text" v-model="username" placeholder="Username" class="forms" />
    <input type="password" v-model="password" placeholder="Password" class="forms" />
    <p v-if="error" class="error">{{error}}</p>
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
      error: "",
    };
  },
  methods: {
    login() {
      this.error = "";
      if (this.username.length < 6 || this.password.length < 8) {
        this.error = "Neispravni podaci";
        return;
      }
      if (this.username.length >= 6) {
        (this.error = ""),
          axios
            .post("http://localhost:3000/login", {
              username: this.username.trim(),
              password: this.password,
            })
            .then((res) => {
              if (res.data.result == "ERROR") {
                this.error = "Neispravni podaci";
              } else {
                localStorage.setItem("user_id", res.data.data.user_id);
                localStorage.setItem("username", res.data.data.username);
                this.$emit("success", res.data.data.user_id);
                this.$emit("close-modal");
                this.$emit("current-user", res.data.data.username);
              }
            });
      } else {
        this.error = "Neispravni podaci";
      }

      // .catch((err) => {
      //   console.log(err.message);
      // });
    },
  },
};
</script>

<style scoped>
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
  /* background-color: rgba(56, 21, 13, 0.274); */
  background-color: rgba(156, 116, 98, 0.185);
  border: rgba(56, 21, 13, 0.068) 1px solid;
  outline: none;
}
.button {
  width: 263px;
  height: 35px;
  margin-bottom: 30px;
  color: rgb(255, 255, 255);
  background-color: rgba(56, 21, 13, 0.534);
  padding: 5px;
  border: none;
  box-shadow: 3px 3px 3px 0px rgba(0, 0, 0, 0.699);
  outline: none;
}
.button:hover {
  background-color: rgb(23, 112, 112);
}
.error {
  color: red;
  font-size: 12px;
  width: 250px;
  margin: 0 0 10px 20px;
}
</style>
