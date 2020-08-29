<template>
  <div class="header">
    <div class="logo">
      <img class="logo" src="../assets/logo.png" alt="logo-slika" />
    </div>
    <!--end logo-->
    <div class="quote">
      <p>"The Earth without art is just eh"</p>
    </div>
    <div id="nav">
      <router-link to="/">Home</router-link> | <span @click="login">Login </span> |
      <span @click="register">Register</span> |
      <!-- <router-link to="/login">login</router-link> |
        <router-link to="/register">register</router-link> |-->
      <router-link to="/order">Order</router-link>
    </div>
  </div>
</template>

<script>
export default {
  data: function() {
    return {};
  },
  methods: {
    login() {
      console.log("aa");
    },
    register() {
      console.log("aa");
    },
  },
};
</script>

<style>
.header {
  background-color: rgba(56, 21, 13, 0.534);
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
  color: #ffffff;
}

#nav a.router-link-exact-active {
  color: #72dfae;
}
.logo img {
  max-width: 200px;
}
.logo {
  margin-left: 5px;
}
.quote p {
  color: rgb(255, 255, 255);
  font-size: 20px;
  font-family: cursive;
  font-style: italic;
}
</style>
