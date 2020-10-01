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
  /* margin-top: 50px; */
  /* display: grid; */
  /* grid-template-columns: 1fr 1fr 1fr;
  grid-gap: 50px 30px; */
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  justify-content: space-around;
}

@media screen and (max-width: 475px) {
  .home {
    width: 400px;
    padding: 30px;
  }
}
@media screen and (min-width: 475px) and (max-width: 600px) {
  .home {
    width: 450px;
    padding: 30px;
    align-items: center;
  }
}
@media screen and (min-width: 600px) and (max-width: 992px) {
  .home {
    /* width: px; */
    padding: 30px;
    align-items: center;
  }
}
</style>
