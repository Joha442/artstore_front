<template>
  <div>
    <div class="home">
      <div v-for="product in products" :key="product.pro_id">
        <Product :product="product" />
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import axios from "axios";
import Product from "../components/Product.vue";

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
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  justify-content: space-around;
  margin: 50px 5px;
}
</style>
