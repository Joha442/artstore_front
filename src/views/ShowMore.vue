<template>
  <div v-if="currentProduct" class="wrapper">
    <div class="image-order">
      <div>
        <img class="bigImage" :src="currentProduct.pro_image" />
      </div>
      <div class="order">
        <h1>{{currentProduct.pro_title}}</h1>
        <h2>{{currentProduct.aut_fullName}}</h2>
        <p class="price">
          <i class="fas fa-euro-sign"></i>
          {{currentProduct.pro_price}}
        </p>
        <div class="quantity">
          <input type="number" min="1" max="100" step="1" v-model="quantity" />
        </div>
        <button @click.prevent="addToCart" class="cart">DODAJ U KORPU</button>
      </div>
    </div>
    <h3>
      <i class="fas fa-image"></i> O slici
    </h3>
    <div class="about-artwork">
      <p>{{currentProduct.pro_description}}</p>
    </div>
    <h3>
      <i class="fas fa-bookmark"></i>
      {{currentProduct.aut_fullName}}
    </h3>
    <div class="about-author">{{currentProduct.aut_description}}</div>
    <div class="line"></div>
    <p
      class="numberOfComments"
    >{{numberOfComments %10==1 && (numberOfComments%100) !== 11 ? numberOfComments+" Komentar" : numberOfComments + " Komentara" }}</p>
    <!-- <p class="numberOfComments">{{ numberOfComments + " Komentara"}}</p> -->
    <div class="all-comments">
      <Comments
        :pro_id="pro_id"
        :loggedInUserId="loggedInUserId"
        @comment-added="getNumberOfComments"
        @comment-deleted="getNumberOfComments"
      />
    </div>
  </div>
</template>
<script>
import axios from "axios";
import Comments from "../components/Comments.vue";
import router from "../router/index.js";

export default {
  components: {
    Comments,
  },
  props: ["loggedInUserId"],
  data: function () {
    return {
      pro_id: this.$route.params.pro_id,
      currentProduct: null,
      numberOfComments: 0,
      quantity: 1,
    };
  },
  methods: {
    getNumberOfComments() {
      axios
        .get("http://localhost:3000/countcomments?pro_id=" + this.pro_id)
        .then((res) => {
          this.numberOfComments = res.data.data;
        });
    },
    addToCart() {
      if (!this.loggedInUserId) {
        this.$emit("open-login");
      } else {
        this.$emit("add-to-cart", {
          ...this.currentProduct,
          quantity: parseInt(this.quantity),
        });
        router.push({ path: "/order" });
      }
    },
  },
  created: function () {
    axios
      .get("http://localhost:3000/products?pro_id=" + this.pro_id)
      .then((res) => {
        if (res.data.data[0]) {
          this.currentProduct = res.data.data[0];
        }
      });
    this.getNumberOfComments();
  },
};
</script>
<style scoped>
.wrapper {
  width: 1000px;
  margin: 0 auto;
  display: flex;
  flex-direction: column;
}
.image-order {
  justify-content: space-between;
  margin: 50px 0;
  padding: 20px;
  display: flex;
  /* flex-direction: row; */
  background-color: rgba(56, 21, 13, 0.1);
  border-radius: 5px;
  border-bottom: 2px solid rgba(56, 21, 13, 0.1);
}
.quantity {
  display: flex;
  flex-direction: row;
}
.bigImage {
  width: 600px;
}
.order h1,
.order h2,
.price {
  color: rgb(47, 85, 85);
  text-align: left;
}
.order h1 {
  margin: 50px 0 20px;
  font-size: 26px;
}
.order h2 {
  margin: 30px 0;
  font-size: 22px;
  font-weight: normal;
}
.price {
  font-size: 16px;
  font-weight: bold;
  margin: 20px 0;
}
.quantity input {
  /* font-family: FontAwesome, Arial, Helvetica, sans-serif; */
  margin: 10px 0 40px;
  padding: 5px;
  width: 60px;
  height: 30px;
  border-radius: 5px;
  background-color: rgba(56, 21, 13, 0.212);
  border: rgba(56, 21, 13, 0.068) 1px solid;
  text-align: center;
  font-size: 16px;
  outline: none;
}

input[type="number"] {
  position: relative;
}

input[type="number"]::-webkit-inner-spin-button,
input[type="number"]::-webkit-outer-spin-button {
  opacity: 0.5;
  position: absolute;
  top: 0;
  right: 0;
  height: 100%;
}
h3 {
  font-style: italic;
  color: rgb(43, 38, 39);
  text-align: left;
  font-size: 18px;
  margin-left: 10px;
}
.fa-bookmark,
.fa-image {
  /* color: rgba(56, 21, 13, 0.534); */
  color: rgba(12, 96, 102, 0.945);
  font-size: 30px;
}
.about-artwork,
.about-author {
  border-bottom: 2px solid rgba(56, 21, 13, 0.1);
  margin: 20px 0;
  background-color: rgba(56, 21, 13, 0.1);
  border-radius: 5px;
  padding: 15px;
  text-align: justify;
}
.cart {
  color: rgb(255, 255, 255);
  background-color: rgba(56, 21, 13, 0.534);
  padding: 10px;
  border: none;
  box-shadow: 3px 3px 3px 0px rgba(0, 0, 0, 0.699);
  width: 300px;
  outline: none;
}
.cart:active {
  margin-top: -1px;
  opacity: 0.85;
}
.cart:hover {
  background-color: rgb(23, 112, 112);
}
.line {
  border-bottom: 1px solid rgba(56, 21, 13, 0.534);
  margin-bottom: 10px;
}
.all-comments {
  margin-bottom: 50px;
}
.numberOfComments {
  font-style: italic;
  color: rgba(83, 80, 80, 0.856);
  text-align: right;
  padding: 10px 20px;
}
</style>
