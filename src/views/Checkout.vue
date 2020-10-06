<template>
  <div class="order">
    <h1>KORPA</h1>
    <div v-if="!cart.length" class="empty-cart">
      <div>
        <span><i class="fas fa-shopping-cart"></i></span>
        <span class="number-items">{{ numberOfItems }}</span>
      </div>
      <p>Vaša korpa je prazna.</p>
      <button class="cart" @click="showProducts">POGLEDAJTE PROIZVODE</button>
    </div>
    <CartItem
      v-for="item in cart"
      :key="item.pro_id"
      :item="item"
      @qty-increment="quantityIncrement"
      @qty-decrement="quantityDecrement"
      @delete-row="deleteRow"
    />
    <p class="totals" v-if="sum">
      UKUPAN IZNOS:
      <span>EUR {{ sum }},00</span>
    </p>
    <div class="sum" v-if="sum">
      <button class="cart" @click="showProducts">
        <i class="fas fa-caret-left"></i>
        NASTAVITE KUPOVINU
      </button>
      <div class="but-container">
        <button class="cart del" @click="$emit('delete-all')">
          <i class="fas fa-trash"></i>
        </button>
        <div>
          <button class="cart confirm" @click="$emit('open-modal', 'checkout')">
            POTVRDI PORUDŽBINU
          </button>
        </div>
      </div>
    </div>
    <div class="items">
      <!-- <button @click="fetchitems">ff</button> -->
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
    };
  },
  watch: {
    numberOfItems: function () {
      this.total();
    },
  },
  props: ["cart", "numberOfItems"],
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
    // fetchitems(){
    //  axios
    //     .get("http://localhost:3000/items?item_id=" + this.pro_id)
    //     .then((res) => {

    //     });
    // },
    // }
  },
};
</script>
<style scoped>
.order {
  min-height: 85vh;
}
h1 {
  padding: 20px;
  color: rgb(71, 68, 68);
  font-weight: normal;
  text-align: center;
  font-size: 22px;
}
.empty-cart {
  display: flex;
  flex-direction: column;
  text-align: center;
}
.empty-cart p {
  padding: 10px;
}
.empty-cart button {
  margin: 20px auto;
}
.sum {
  width: 90%;
  display: flex;
  margin: 10px auto;
  justify-content: space-between;
}
.totals {
  width: 90%;
  margin: 70px auto 10px;
  color: rgba(0, 0, 0, 0.925);
  text-align: right;
  font-size: 18px;
}
.totals span {
  font-weight: bold;
  font-size: 25px;
}
.but-container {
  display: flex;
}
.cart {
  color: rgb(255, 255, 255);
  background-color: rgba(56, 21, 13, 0.534);
  padding: 10px;
  border: none;
  box-shadow: 3px 3px 3px 0px rgba(0, 0, 0, 0.699);
  width: 250px;
  outline: none;
  text-align: center;
  margin: 10px 0;
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
  margin-right: 5px;
  width: 80px;
  /* background-color: rgba(255, 0, 0, 1); */
  background-color: rgba(231, 57, 18, 0.534);
}
.del:hover {
  background-color: rgb(255, 0, 0);
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
.confirm {
  background-color: rgba(0, 128, 0, 0.726);
}
.confirm:hover {
  background-color: rgba(0, 128, 0, 0.692);
}
@media screen and (max-width: 760px) {
  .totals {
    text-align: left;
    margin-left: 35px;
  }
  .sum {
    flex-direction: column;
    justify-items: left;
  }
}
@media screen and (max-width: 540px) {
  .sum {
    width: 95%;
  }
  .totals span {
    font-size: 22px;
  }
}
</style>
