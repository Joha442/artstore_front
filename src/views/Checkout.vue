<template>
  <div class="order">
    <!-- <div> -->
    <h1>Vaša korpa</h1>
    <div v-if="cart.length" class="title">
      <p class="price">Cena</p>
      <p class="qty">Količina</p>
      <p class="total">Ukupno</p>
    </div>
    <div v-if="!cart.length">
      <i class="fas fa-shopping-cart"></i>
      <span class="number-items">{{numberOfItems}}</span>
      <p class="empty-cart">Vasa korpa je prazna.</p>
      <button class="cart" @click="showProducts">POGLEDAJTE PROIZVODE</button>
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
        UKUPAN IZNOS: {{sum}}
        <i class="fas fa-euro-sign"></i>
      </p>
      <button class="cart" @click="$emit('open-modal','checkout' )">
        <i class="far fa-check-square"></i> POTVRDI PORUDŽBINU
      </button>
    </div>
    <div class="btn" v-if="sum">
      <button class="cart" @click="showProducts">
        <i class="fas fa-caret-left"></i>
        NASLOVNA STRANA
      </button>
      <button class="cart del" @click="$emit('delete-all')">
        <i class="fas fa-trash"></i>
      </button>
    </div>
    <!-- </div> -->
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
    };
  },
  props: ["cart", "numberOfItems"],
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
      console.log("UPDATE");
      var sum = 0;
      for (let i = 0; i < this.cart.length; i++) {
        sum += this.cart[i].pro_price * this.cart[i].quantity;
      }
      this.sum = sum;
    },
    showProducts() {
      router.push({ path: "/" });
    },
  },
};
</script>
<style scoped>
.title {
  display: flex;
  flex-direction: row;
  margin: 10px 0;
  color: rgb(75, 74, 74);
  font-style: italic;
}
h1 {
  color: rgba(0, 0, 0, 0.664);
  font-size: 24px;
  padding: 10px;
  text-align: center;
}
.price {
  margin-left: 900px;
}
.qty {
  margin: 0 30px 0 70px;
}
.sum {
  font-weight: bold;
  font-style: italic;
  text-align: right;
  margin: 30px 280px;
  padding: 20px;
  background-color: rgba(56, 21, 13, 0.1);
}
.sum p {
  margin: 20px;
}
.btn {
  display: flex;
  justify-content: flex-end;
  margin: 0 280px 0 0;
}
.btn button {
  margin: 10px;
}
.cart {
  color: rgb(255, 255, 255);
  background-color: rgba(56, 21, 13, 0.534);
  padding: 10px;
  border: none;
  box-shadow: 3px 3px 3px 0px rgba(0, 0, 0, 0.699);
  width: 200px;
  outline: none;
  text-align: center;
}
.cart:active {
  /* margin-top: -10px; */
  opacity: 0.85;
}
.cart:hover {
  background-color: rgb(23, 112, 112);
}
.fa-shopping-cart {
  font-size: 50px;
  color: rgb(23, 112, 112);
}
.del {
  width: 50px;
}
.number-items {
  display: inline-block;
  color: rgb(23, 112, 112);
  font-size: 50px;
  border: 2px solid rgb(23, 112, 112);
  border-radius: 999px;
  margin: 10px;
  width: 70px;
}
</style>
