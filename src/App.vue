<template>
  <div id="app">
    <Header
      @open-modal="changeWhichForm"
      :loggedInUserId="loggedInUserId"
      :numberOfItems="numberOfItems"
      :currentUser="currentUser"
    />
    <div class="quote">
      <p>Prodaja kopija poznatih umetničkih dela</p>
    </div>
    <Modal
      :whichForm="whichForm"
      @close-modal="clearWhichForm"
      @open-login="openLogin"
      @success="loginUser"
      @logout="logoutUser"
      @current-user="currentUserUsername"
    />
    <router-view
      :loggedInUserId="loggedInUserId"
      @add-to-cart="addToCart"
      :cart="cart"
      @delete-row="deleteProduct"
      @qty-increment="productIncrement"
      @qty-decrement="productDecrement"
      @open-modal="changeWhichForm"
    />
    <footer>
      <p>Jovana Stojanovic, 2020</p>
    </footer>
  </div>
</template>
<script>
import Header from "./components/Header.vue";
import Modal from "./components/Modal.vue";
export default {
  components: {
    Header,
    Modal,
  },
  data() {
    return {
      whichForm: "",
      loggedInUserId: parseInt(localStorage.getItem("user_id")),
      cart: [],
      numberOfItems: 0,
      currentUser: localStorage.getItem("username"),
    };
  },
  methods: {
    changeWhichForm(whichForm) {
      this.whichForm = whichForm;
    },
    clearWhichForm() {
      this.whichForm = "";
    },
    loginUser(user_id) {
      this.loggedInUserId = user_id;
    },
    logoutUser() {
      this.loggedInUserId = "";
      this.currentUser = "";
    },
    openLogin() {
      this.whichForm = "login";
    },
    currentUserUsername(username) {
      this.currentUser = username;
    },
    itemsNumber() {
      this.numberOfItems = 0;
      for (var i = 0; i < this.cart.length; i++) {
        this.numberOfItems += this.cart[i].quantity;
      }
    },
    addToCart(currentProduct) {
      for (var i = 0; i < this.cart.length; i++) {
        if (this.cart[i].pro_id === currentProduct.pro_id) {
          this.cart[i].quantity += currentProduct.quantity;
          return;
        }
      }
      this.cart.push(currentProduct);
      this.itemsNumber();
    },
    deleteProduct(pro_id) {
      var index = null;
      for (let i = 0; i < this.cart.length; i++) {
        if (this.cart[i].pro_id == pro_id) {
          index = i;
          break;
        }
      }
      this.cart.splice(index, 1);
      this.itemsNumber();
    },
    productIncrement(pro_id) {
      for (let i = 0; i < this.cart.length; i++) {
        if (this.cart[i].pro_id == pro_id) {
          this.cart[i].quantity++;
        }
      }
      this.itemsNumber();
    },
    productDecrement(pro_id) {
      for (let i = 0; i < this.cart.length; i++) {
        if (this.cart[i].pro_id == pro_id) {
          this.cart[i].quantity--;
        }
        if (this.cart[i].quantity == 0) {
          this.deleteProduct(pro_id);
        }
      }
      this.itemsNumber();
    },
  },
};
</script>

<style>
html,
body,
p,
h1,
h2,
h3 {
  padding: 0;
  margin: 0;
  font-family: "Open Sans", Arial, Helvetica, sans-serif;
}
#app {
  /* -webkit-font-smoothing: antialiased; */
  /* -moz-osx-font-smoothing: grayscale; */
  text-align: center;
}
.quote {
  background-color: rgb(23, 112, 112);
  padding: 2px;
}
.quote p {
  color: rgb(255, 255, 255);
  font-size: 15px;
  font-style: italic;
}
footer {
  padding: 15px;
  background-color: rgba(56, 21, 13, 0.534);
  margin-top: 20px;
}
footer p {
  margin: 0;
  text-align: center;
  color: white;
}
</style>
