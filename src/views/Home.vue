<template>
  <div>
    <div class="home">
      <Product v-for="product in products" :key="product.pro_id" :product="product" />
    </div>
  </div>
</template>

<script>
// <PopUp :show="show" :form:"whichForm">
// Njena Unutrasnjost...

// template
//   Login v-if form=login
//   Register    vif form=register
//   Logout    vif form=logout
import axios from "axios";
import Product from "../components/Product.vue";
import LoginVue from "./Login.vue";

export default {
  name: "Home",
  components: {
    Product,
  },
  data: function() {
    return {
      products: [],
    };
  },
  created: function() {
    axios.get("http://localhost:3000/products").then((res) => {
      this.products = res.data.data;
    });
  },
};
</script>
<style scoped>
.home {
  width: 1200px;
  margin: 50px auto;
  /* display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  justify-content: space-around; */
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;

  justify-items: center;
  gap: 40px 40px;
  /* margin: 50px 40px 200px 0px; */
}
</style>
