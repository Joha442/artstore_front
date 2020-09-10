<template>
  <div v-if="whichForm" class="modal-bg">
    <div class="modal">
      <button @click="$emit('close-modal')" class="closeBtn">X</button>
      <Login
        v-if="whichForm==='login'"
        @close-modal="$emit('close-modal')"
        @success="continueEmit"
      />
      <Register v-if="whichForm==='register'" @open-login="$emit('open-login')" />
      <div class="logout" v-if="whichForm==='logout'">
        <p>Are you sure?</p>
        <button @click="logout" class="logoutBtn">Yes</button>
      </div>
    </div>
  </div>
</template>
<script>
import Login from "../components/Login.vue";
import Register from "../components/Register.vue";
export default {
  props: ["whichForm"],
  components: {
    Login,
    Register,
  },
  methods: {
    logout() {
      this.$emit("close-modal");
      this.$emit("logout");
      localStorage.removeItem("user_id");
    },
    continueEmit(user_id) {
      this.$emit("success", user_id);
    },
  },
};
</script>
<style scoped>
.modal-bg {
  position: fixed;
  top: 0;
  left: 0;
  height: 100%;
  width: 100%;
  background-color: rgba(0, 0, 0, 0.7);
  display: flex;
  justify-content: center;
  align-items: center;
}
.modal {
  width: 300px;
  position: fixed;
  top: 40%;
  left: 40%;
  z-index: 1000;
  background-color: rgb(255, 255, 255);
}
.closeBtn {
  position: relative;
  top: 10px;
  left: 110px;
  width: 40px;
  color: rgb(255, 255, 255);
  background-color: rgba(56, 21, 13, 0.534);
  padding: 5px;
  border: none;
  box-shadow: 3px 3px 3px 0px rgba(0, 0, 0, 0.699);
}
.closeBtn:hover {
  background-color: rgb(23, 112, 112);
}
.logout {
  width: 300px;
  height: 80px;
}
.logoutBtn {
  width: 60px;
  margin-top: 15px;
  color: rgb(255, 255, 255);
  background-color: rgba(56, 21, 13, 0.534);
  padding: 5px;
  border: none;
  box-shadow: 3px 3px 3px 0px rgba(0, 0, 0, 0.699);
}
.logoutBtn:hover {
  background-color: rgb(23, 112, 112);
}
</style>