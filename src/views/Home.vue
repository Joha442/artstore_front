<template>
  <div>
    <div class="home">
      <Product
        v-for="product in products"
        :key="product.pro_id"
        :product="product"
      />
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
  width: 90%;
  margin: 30px auto;
  display: grid;
  grid-template-columns: 1fr 1fr 1fr 1fr;
  grid-gap: 25px 25px;
  justify-content: center;
}

@media screen and (max-width: 1200px) {
  .home {
    margin-top: 25px;
    grid-template-columns: 1fr 1fr 1fr;
    /* grid-gap: 20px 20px; */
  }
}
@media screen and (max-width: 992px) {
  .home {
    /* width: 90%; */
    /* margin-top: 25px; */
    grid-template-columns: 1fr 1fr;
    grid-gap: 30px 30px;
  }
}
@media screen and (max-width: 590px) {
  .home {
    width: 100%;
    margin-top: 10px;
    grid-template-columns: 1fr;
    grid-gap: 10px 0;
    /* margin: 10px auto; */
  }
}
</style>
