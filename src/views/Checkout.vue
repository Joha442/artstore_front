<template>
  <div class="order">
    <h1>Vaša korpa</h1>
    <div class="container" v-if="cart.length">
      <div class="imgtitle">
        <div class="image" />
        <div class="title" />
      </div>
      <div class="qtyprice">
        <div class="price">Cena</div>
        <div class="qty">Količina</div>
        <div class="total">Ukupno</div>
        <div class="delete"></div>
      </div>
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
        UKUPAN IZNOS:
        <span>
          {{sum}}
          <i class="fas fa-euro-sign"></i>
        </span>
      </p>
      <button class="cart" @click="$emit('open-modal','checkout' )">POTVRDI PORUDŽBINU</button>
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
  },
};
</script>
<style scoped>
.container {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  padding: 10px;
  margin: 0 auto;
  width: 90%;
}
.imgtitle {
  display: flex;
  flex-direction: row;
}
.image {
  width: 250px;
}
.title {
  text-align: left;
  margin: auto 10px;
  font-size: 20px;
}
.qtyprice {
  display: flex;
}
.price {
  margin: auto 10px;
}
.qty {
  margin: auto 10px;
}
.total {
  margin: auto 10px;
}
.delete {
  margin: auto 10px;
}
.sum {
  text-align: right;
  margin: 30px 280px;
  padding: 10px;
  /* background-color: rgba(56, 21, 13, 0.1); */
}
.sum p {
  margin: 10px;
  color: rgba(0, 0, 0, 0.925);
}
.sum span {
  font-size: 25px;
  font-weight: bold;
  color: black;
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
  width: 250px;
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
