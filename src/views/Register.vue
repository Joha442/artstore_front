<template>
  <div>
    <form @submit.prevent="register">
      <input type="text" v-model="username" placeholder="Username" class="forms" />
      <input type="password" v-model="password" placeholder="Password" class="forms" />
      <input type="password" v-model="confirmPassword" placeholder="Confirm Password" class="forms" />
      <p v-if="error" class="error">{{ error }}</p>
      <button type="submit" class="forms button">Register</button>
    </form>
  </div>
</template>
<script>
import axios from "axios";
import router from "../router";

export default {
  data: function () {
    return {
      username: "",
      password: "",
      confirmPassword: "",
      error: "",
    };
  },
  methods: {
    register() {
      if (this.password === this.confirmPassword) {
        this.error = "";
        axios
          .post("http://localhost:3000/register", {
            username: this.username.trim(),
            password: this.password,
          })
          .then((res) => {
            router.push("/");
          });
      } else {
        this.error = "Passwords don't match";
      }
    },
  },
};
</script>
<style scoped>
.error {
  color: red;
  font-size: 10px;
  width: 250px;
}
</style>
