<template>
  <div class="order">
    <div>
      <h1>Vaša korpa</h1>
      <div v-if="cart.length" class="title">
        <p class="price">Cena</p>
        <p class="qty">Količina</p>
        <p class="total">Ukupno</p>
      </div>
      <div v-if="!cart.length">
        <p class="empty-cart">Vasa korpa je prazna.</p>
        <button class="cart" @click="showProducts">Pogledajte proizvode</button>
      </div>
      <CartItem
        v-for="item in cart"
        :key="item.pro_id"
        :item="item"
        @qty-increment=" quantityIncrement"
        @qty-decrement=" quantityDecrement"
        @delete-row=" deleteRow"
      />
      <div class="sum" v-if="sum">
        <p>
          IZNOS: {{sum}}
          <i class="fas fa-euro-sign"></i>
        </p>
        <button class="cart" @click="$emit('open-modal','checkout' )">Nastavi kupovinu</button>
      </div>
    </div>
  </div>
</template>
<script>
import axios from "axios";
import CartItem from "../components/CartItem.vue";
import router from "../router/index.js";

export default {
  components: {
    CartItem,
  },
  data: function () {
    return {
      sum: 0,
      address: "",
      country: "",
      city: "",
      postcode: "",
    };
  },
  props: ["cart"],
  updated() {
    this.total();
  },
  created() {
    this.total();
  },
  methods: {
    deleteRow(pro_id) {
      this.$emit("delete-row", pro_id);
    },
    quantityIncrement(pro_id) {
      this.$emit("qty-increment", pro_id);
    },
    quantityDecrement(pro_id) {
      this.$emit("qty-decrement", pro_id);
    },
    total() {
      var sum = 0;
      for (let i = 0; i < this.cart.length; i++) {
        sum += this.cart[i].pro_price * this.cart[i].quantity;
      }
      this.sum = sum;
    },
    showProducts() {
      router.push({ path: "/" });
    },
    checkout() {
      axios
        .post("http://localhost:3000/orders", {
          ord_adress: this.address,
          ord_city: this.city,
          ord_country: this.country,
          ord_postcode: this.postcode,
          user_id: localStorage.getItem("user_id"),
        })
        .then((res) => {
          console.log(res.data);
        });
    },
  },
};
</script>
<style scoped>
.order {
  display: flex;
  justify-content: space-around;
}
.title {
  display: flex;
  flex-direction: row;
  margin: 20px;
  color: rgb(75, 74, 74);
  font-style: italic;
}
h1 {
  color: rgba(0, 0, 0, 0.664);
  font-size: 24px;
  padding: 10px;
  margin: 10px;
  text-align: left;
}
.price {
  margin-left: 390px;
}
.qty {
  margin: 0 80px;
}
.sum {
  float: right;
  margin-right: 100px;
  font-weight: bold;
  font-style: italic;
}
.empty-cart {
  /* margin: 20px 50px; */
  text-align: center;
}
.cart {
  color: rgb(255, 255, 255);
  background-color: rgba(56, 21, 13, 0.534);
  padding: 10px;
  border: none;
  box-shadow: 3px 3px 3px 0px rgba(0, 0, 0, 0.699);
  width: 300px;
  outline: none;
  margin-top: 10px;
}
.cart:active {
  margin-top: -2px;
  opacity: 0.85;
}
.cart:hover {
  background-color: rgb(23, 112, 112);
  /* margin-top: -px; */
}
.forms {
  font-family: FontAwesome, Arial, Helvetica, sans-serif;
  display: block;
  margin: 20px;
  padding: 5px;
  width: 250px;
  height: 25px;
  background-color: rgba(56, 21, 13, 0.274);
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
