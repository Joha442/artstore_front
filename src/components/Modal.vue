<template>
  <div v-if="whichForm" class="modal-bg">
    <div class="modal">
      <button @click="$emit('close-modal')" class="closeBtn">X</button>
      <Login
        v-if="whichForm === 'login'"
        @close-modal="$emit('close-modal')"
        @success="continueIDEmit"
        @current-user="continueUsernameEmit"
        @open-register="$emit('open-register')"
      />
      <Register
        v-if="whichForm === 'register'"
        @open-login="$emit('open-login')"
      />
      <div class="logout" v-if="whichForm === 'logout'">
        <p>Da li ste sigurni da želite da se odjavite?</p>
        <button @click="logout" class="logoutBtn">Da</button>
      </div>
      <OrderForm
        v-if="whichForm === 'checkout'"
        :cart="cart"
        @close-modal="$emit('close-modal')"
        @clear-cart="$emit('clear-cart')"
      />
    </div>
  </div>
</template>
<script>
import Login from "../components/Login.vue";
import Register from "../components/Register.vue";
import OrderForm from "../components/OrderForm.vue";
export default {
  props: ["whichForm", "cart"],
  components: {
    Login,
    Register,
    OrderForm,
  },
  methods: {
    logout() {
      this.$emit("close-modal");
      this.$emit("logout");
      localStorage.removeItem("user_id");
      localStorage.removeItem("username");
    },
    continueIDEmit(user_id) {
      this.$emit("success", user_id);
    },
    continueUsernameEmit(username) {
      this.$emit("current-user", username);
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
  background-color: rgba(0, 0, 0, 0.808);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 999;
}
.modal {
  width: 300px;
  position: fixed;
  z-index: 1000;
  background-color: rgb(255, 255, 255);
  animation: modalopen 1.5s;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  text-align: center;
}
@keyframes modalopen {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}
.closeBtn {
  position: relative;
  top: 10px;
  left: 40%;
  width: 40px;
  color: rgb(255, 255, 255);
  background-color: rgba(56, 21, 13, 0.534);
  padding: 5px;
  border: none;
  box-shadow: 3px 3px 3px 0px rgba(0, 0, 0, 0.699);
  outline: none;
}
.closeBtn:active {
  margin-top: -1px;
  opacity: 0.85;
}
.closeBtn:hover {
  background-color: rgb(23, 112, 112);
}
.logout {
  width: 300px;
  /* padding: 30px; */
}
.logout p {
  margin: 40px 0 20px;
  text-align: center;
  font-size: 18px;
  /* border: black 2px solid; */
}
.logoutBtn {
  width: 80px;
  color: rgb(255, 255, 255);
  background-color: rgba(56, 21, 13, 0.534);
  padding: 5px;
  border: none;
  box-shadow: 3px 3px 3px 0px rgba(0, 0, 0, 0.699);
  outline: none;
  margin-bottom: 20px;
}
.logout:active {
  margin-top: -1px;
  opacity: 0.85;
}
.logoutBtn:hover {
  background-color: rgb(23, 112, 112);
}
</style>