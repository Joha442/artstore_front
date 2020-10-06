<template>
  <div class="header">
    <div class="logo">
      <router-link to="/">
        <img class="logo" src="../assets/logo.png" alt="logo-image" />
      </router-link>
    </div>

    <div id="nav">
      <p v-if="!loggedInUserId" @click="$emit('open-modal', 'login')">
        <span class="space"> </span><i class="fas fa-user"></i
        ><span class="user"> Prijava |</span>
      </p>
      <p v-if="!loggedInUserId" @click="$emit('open-modal', 'register')">
        <span class="space"> </span><i class="fas fa-user-plus"></i
        ><span class="user"> Registracija |</span>
      </p>
      <p v-if="loggedInUserId" @click="$emit('open-modal', 'logout')">
        <span class="space"> </span><i class="fas fa-user-alt"></i>
        <!-- <span class="hello"> Zdravo, {{ currentUser }} |</span> -->
        <span><i class="fas fa-power-off"></i> </span>
        <span class="logout"> Odjavi se |</span>
      </p>
      <p>
        <router-link to="/order">
          <span class="space"> </span><i class="fas fa-shopping-cart"></i>
          <span> {{ numberOfItems }} </span>
        </router-link>
      </p>
    </div>
  </div>
</template>

<script>
export default {
  data: function () {
    return {
      logout: false,
    };
  },
  props: ["loggedInUserId", "numberOfItems", "currentUser"],
  methods: {
    showLogout() {
      this.logout = !this.logout;
    },
  },
};
</script>

<style scoped>
.header {
  background-color: rgba(56, 21, 13, 0.534);
  display: flex;
  justify-content: space-between;
  align-items: center;
}
#nav {
  display: flex;
  padding: 20px;
}
#nav a {
  font-size: 20px;
  color: #ffffff;
  text-decoration: none;
}
#nav a.router-link-exact-active {
  color: rgb(23, 112, 112);
}
p {
  color: rgb(255, 255, 255);
  font-size: 20px;
  cursor: pointer;
}

.space {
  padding-right: 5px;
}
.logo img {
  max-width: 200px;
  margin-bottom: -9px;
}
.logo {
  margin-left: 5px;
}
.fa-user-alt {
  color: rgb(75, 196, 126);
  display: none;
}
.fa-power-off {
  /* display: none; */
  /* color: rgb(104, 0, 0); */
  transform: rotate(90deg);
}
@media screen and (max-width: 620px) {
  #nav {
    padding: 8px 8px 2px;
  }
  #nav a,
  p {
    font-size: 20px;
  }
  .logo img {
    max-width: 150px;
    margin-left: 2px;
    margin-bottom: -6px;
  }
  .fa-house-user,
  .fa-user-plus,
  .user,
  .logout {
    display: none;
  }
}
</style>
