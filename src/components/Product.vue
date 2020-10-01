<template>
  <div class="container">
    <router-link :to="{ name: 'ShowMore', params: { pro_id: product.pro_id } }">
      <img :src="product.pro_image" />
      <h1>{{ product.pro_title }}</h1>
      <h2>{{ product.aut_fullName }}</h2>
      <div class="price-comment">
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
};
</script>
<style scoped>
.container {
  width: 346px;
  border-bottom: rgba(156, 116, 98, 0.185) 2px solid;
  border-bottom-left-radius: 5px;
  border-bottom-right-radius: 5px;
  background-color: rgba(156, 116, 98, 0.185);
  transition: 1s;
  margin-bottom: 50px;
}
img {
  width: 100%;
}
.container:hover {
  transform: scale(1.2);
  /* z-index: 2; */
}
.container h1,
.container h2,
.container h3,
.fa-comment-alt {
  color: rgba(43, 44, 44, 0.938);
  font-weight: normal;
}
.container h1 {
  font-size: 20px;
  margin: 10px;
}
.container h2 {
  font-size: 18px;
  font-style: italic;
  margin: 20px;
}
.container h3 {
  font-size: 16px;
}
a {
  text-decoration: none;
}
.fa-comment-alt {
  font-size: 20px;
}
.price-comment {
  display: flex;
  justify-content: space-around;
  margin-bottom: 20px;
}
@media screen and (min-width: 475px) and (max-width: 600px) {
  .container {
    width: 400px;
  }
}
@media screen and (min-width: 600px) and (max-width: 992px) {
  .container {
    width: 450px;
  }
}
</style>
