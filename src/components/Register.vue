<template>
  <div>
    <form @submit.prevent="register">
      <h1>
        <i class="fas fa-user-plus"></i>
      </h1>
      <input type="text" v-model="username" placeholder="Username" class="forms" />
      <input type="password" v-model="password" placeholder="Password" class="forms" />
      <input type="password" v-model="confirmPassword" placeholder="Confirm Password" class="forms" />
      <p v-if="error" class="error">{{ error }}</p>
      <button type="submit" class="forms button">REGISTRACIJA</button>
    </form>
  </div>
</template>
<script>
import axios from "axios";

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
            this.$emit("open-login");
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
