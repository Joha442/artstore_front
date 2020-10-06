<template>
  <div>
    <h1>
      <i class="fas fa-user-plus"></i>
    </h1>
    <input
      type="text"
      v-model="username"
      placeholder="Username"
      class="forms"
    />
    <p v-if="errorUsername" class="error">{{ errorUsername }}</p>
    <input
      type="password"
      v-model="password"
      placeholder="Password"
      class="forms"
    />
    <input
      type="password"
      v-model="confirmPassword"
      placeholder="Confirm Password"
      class="forms"
    />
    <p v-if="errorPassword" class="error">{{ errorPassword }}</p>
    <button class="forms button" @click="register">REGISTRACIJA</button>
    <p class="pass">
      *Šifra mora da sadrži najmanje jedno malo slovo, najmanje jedno veliko
      slovo, najmanje 8 karaktera i najmanje jedan broj
    </p>
  </div>
</template>
<script>
import axios from "axios";

export default {
  data: function () {
    return {
      // rules: [
      //   {
      //     message: "Šifra mora da sadrži najmanje jedno malo slovo.",
      //     regex: /[a-z]+/,
      //   },
      //   {
      //     message: "Šifra mora da sadrži najmanje jedno veliko slovo.",
      //     regex: /[A-Z]+/,
      //   },
      //   {
      //     message: "Šifra mora da sadrži najmanje 8 karaktera.",
      //     regex: /.{8,}/,
      //   },
      //   {
      //     message: "Šifra mora da sadrži najmanje jedan broj",
      //     regex: /[0-9]+/,
      //   },
      // ],
      rules: [],
      username: "",
      password: "",
      confirmPassword: "",
      errorUsername: "",
      errorPassword: "",
    };
  },
  methods: {
    register() {
      this.errorPassword = "";
      this.errorUsername = "";
      console.log(this.errorUsername);
      // if (this.username.length < 6) {

      //   this.errorUsername = "Kratak Username";
      //   return;
      // }
      for (let condition of this.rules) {
        if (!condition.regex.test(this.password)) {
          this.errorPassword = condition.message;
          return;
        }
      }

      if (this.password === this.confirmPassword) {
        this.errorPassword = "";
        axios
          .post("http://localhost:3000/register", {
            username: this.username.trim(),
            password: this.password,
          })
          .then((res) => {
            this.$emit("open-login");
          });
      } else {
        this.errorPassword = "Passwords don't match";
      }
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
  margin: 20px auto;
  padding: 5px;
  width: 80%;
  height: 25px;
  background-color: rgba(56, 21, 13, 0.274);
  border: rgba(56, 21, 13, 0.068) 1px solid;
  outline: none;
}
.button {
  width: 84%;
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
.button:active {
  margin-top: -1px;
  opacity: 0.85;
}
.error {
  color: red;
  font-size: 12px;
  width: 250px;
  margin: 0 0 10px 20px;
}
.pass {
  font-size: 10px;
  font-style: italic;
  color: rgba(0, 0, 0, 0.774);
  margin: 10px 10px 30px;
  text-align: justify;
}
</style>
