<template>
  <div class="header">
    <div class="logo">
      <router-link to="/">
        <img class="logo" src="../assets/logo.png" alt="logo-image" />
      </router-link>
    </div>
    <div id="nav">
      <router-link to="/">
        <i class="fas fa-house-user"></i>
      </router-link>
      <span v-if="!loggedInUserId" @click="$emit('open-modal', 'login')">
        |
        <i class="fas fa-user"></i> Prijava |
      </span>
      <span v-if="!loggedInUserId" @click="$emit('open-modal', 'register')">
        <i class="fas fa-user-plus"></i> Registracija |
      </span>
      <span v-if="loggedInUserId" @click="$emit('open-modal', 'logout')">
        &nbsp;|
        <i class="fas fa-user-alt"></i>
        {{currentUser}},
        Odjavi se |
      </span>

      <router-link to="/order">
        <i class="fas fa-shopping-cart"></i>
        {{numberOfItems}}
      </router-link>
    </div>
  </div>
</template>

<script>
export default {
  data: function () {
    return {};
  },
  props: ["loggedInUserId", "numberOfItems", "currentUser"],
  methods: {},
};
</script>

<style scoped>
.header {
  background-color: rgba(56, 21, 13, 0.534);
  box-shadow: 5px 11px 11px -8px rgba(0, 0, 0, 0.8);
  display: flex;
  justify-content: space-between;
  align-items: center;
  /* align-content: center; */
}
#nav {
  padding: 30px;
}
#nav a {
  font-size: 20px;
  color: #ffffff;
  text-decoration: none;
}

#nav a.router-link-exact-active {
  color: rgb(23, 112, 112);
}
.logo img {
  max-width: 200px;
}
.logo {
  margin-left: 5px;
}
span {
  color: rgb(255, 255, 255);
  font-size: 20px;
  cursor: pointer;
}
</style>
