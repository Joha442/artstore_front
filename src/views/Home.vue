<template>
  <div>
    <div class="home">
      <Product v-for="product in products" :key="product.pro_id" :product="product" />
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Product from "../components/Product.vue";

export default {
  name: "Home",
  components: {
    Product,
  },
  data: function () {
    return {
      products: [],
    };
  },
  created: function () {
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
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  justify-items: center;
  grid-gap: 50px 30px;
}
</style>
