<template>
  <div>
    <div class="home">
      <Product
        v-for="product in products"
        :key="product.pro_id"
        :product="product"
        :userLevel="userLevel"
        @delete-product="deleteProduct"
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
  props: ["loggedInUserId"],
  data: function () {
    return {
      products: [],
      userLevel: null,
    };
  },
  created: function () {
    axios.get("http://localhost:3000/products").then((res) => {
      this.products = res.data.data;
    });
    this.getUserLevel();
  },
  watch: {
    loggedInUserId: function () {
      this.getUserLevel();
    },
  },
  methods: {
    getUserLevel() {
      if (this.loggedInUserId) {
        axios
          .get("http://localhost:3000/users?user_id=" + this.loggedInUserId)
          .then((res) => {
            this.userLevel = res.data.data[0].user_level;
          });
      } else {
        this.userLevel = null;
      }
    },
    deleteProduct(pro_id) {
      var index = null;
      for (let i = 0; i < this.products.length; i++) {
        if (this.products[i].pro_id == pro_id) {
          index = i;
          break;
        }
      }
      this.products.splice(index, 1);
    },
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
