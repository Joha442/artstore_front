<template>
  <div v-if="currentProduct" class="wrapper">
    <div class="image-order">
      <div>
        <img class="bigImage" :src="currentProduct.pro_image" />
      </div>
      <div class="order">
        <h1>{{currentProduct.pro_title}}</h1>
        <h2>{{currentProduct.aut_fullName}}</h2>
        <div class="price">
          <i class="fas fa-euro-sign"></i>
          {{currentProduct.pro_price}}
        </div>
        <div class="quantity">Kolicina</div>
        <button @click.prevent="korpa" class="cart">DODAJ U KORPU</button>
      </div>
    </div>
    <h3>
      <i class="fas fa-paint-brush"></i> O slici
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
    <p class="numberOfComments">{{ numberOfComments + " Komentara"}}</p>
    <div class="all-comments">
      <Comments :pro_id="pro_id" :loggedInUserId="loggedInUserId" @comment-deleted="getComments" />
    </div>
  </div>
</template>
<script>
import axios from "axios";
import Comments from "../components/Comments.vue";

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
    };
  },
  created: function () {
    axios
      .get("http://localhost:3000/products?pro_id=" + this.pro_id)
      .then((res) => {
        if (res.data.data[0]) {
          this.currentProduct = res.data.data[0];
        }
      });
    axios
      .get("http://localhost:3000/countcomments?pro_id=" + this.pro_id)
      .then((res) => {
        this.numberOfComments = res.data.data;
      });
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
  flex-direction: row;
  background-color: rgba(56, 21, 13, 0.1);
  border-radius: 5px;
  border-bottom: 2px solid rgba(56, 21, 13, 0.1);
}

.bigImage {
  width: 600px;
}

.order h1 {
  color: rgb(47, 85, 85);
  margin: 50px 0 20px;
  font-size: 26px;
}
.order h2 {
  color: rgb(49, 85, 85);
  font-size: 22px;
  margin: 30px 0;
}
.price {
  color: rgb(49, 85, 85);
  font-size: 16px;
  text-align: left;
  font-weight: bold;
  margin: 20px;
}
.quantity {
  margin: 30px 0;
  padding: 20px;
  width: 280px;
  text-align: left;
}
h3 {
  font-style: italic;
  color: rgb(23, 112, 112);
  text-align: left;
  font-size: 20px;
  margin-left: 10px;
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
