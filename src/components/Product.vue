<template>
  <div class="container">
    <router-link :to="{ name: 'ShowMore', params: { pro_id: product.pro_id } }">
      <img :src="product.pro_image" />
      <h1>{{ product.pro_title }}</h1>
      <h2>{{ product.aut_fullName }}</h2>
      <div class="price-comm">
        <h3>
          {{product.pro_price}}
          <i class="fas fa-euro-sign"></i>
        </h3>
        <span>
          <i class="far fa-comment-alt">{{ " " + numberOfComments }}</i>
        </span>
      </div>
    </router-link>
  </div>
</template>
<script>
import axios from "axios";
export default {
  name: "Product",
  props: ["product"],
  data() {
    return {
      numberOfComments: 0,
    };
  },
  created: function () {
    axios
      .get("http://localhost:3000/countcomments?pro_id=" + this.product.pro_id)
      .then((res) => {
        this.numberOfComments = res.data.data;
      });
  },
  methods: {
    // korpa() {
    //   console.log("bla");
    // },
  },
};
</script>
<style scoped>
.container {
  width: 346px;
  height: 380px;
  transition: 1s;
  border-bottom: rgba(156, 116, 98, 0.185) 2px solid;
  border-bottom-left-radius: 5px;
  border-bottom-right-radius: 5px;
  background-color: rgba(156, 116, 98, 0.185);
}
.container:hover {
  transform: scale(1.2);
  z-index: 2;
}

p {
  padding: 10px;
}
.container h1 {
  color: rgba(43, 44, 44, 0.938);
  font-size: 20px;
  margin: 10px;
  font-weight: normal;
}
.container h2 {
  color: rgba(43, 44, 44, 0.938);
  font-size: 18px;
  font-style: italic;
  margin: 20px;
  font-weight: normal;
}
.container h3 {
  color: rgba(43, 44, 44, 0.938);
  font-size: 16px;
  font-weight: normal;
}
a {
  text-decoration: none;
}
.far,
.fa-comment-alt {
  font-size: 20px;
  color: rgba(43, 44, 44, 0.938);
}
.price-comm {
  display: flex;
  justify-content: space-around;
}
</style>
