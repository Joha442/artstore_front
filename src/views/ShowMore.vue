<template>
  <div v-if="currentProduct" class="wrapper">
    <div class="image-order">
      <div class="bigImage">
        <img :src="currentProduct.pro_image" />
      </div>
      <div class="order">
        <div>
          <h1>{{ currentProduct.pro_title }}</h1>
          <h2>{{ currentProduct.aut_fullName }}</h2>
          <p>
            EUR
            {{ currentProduct.pro_price }},00
          </p>
        </div>
        <!-- <div class="wrapper-add"> -->
        <div class="qty-add">
          <div class="quantity">
            <input type="text" v-model="quantity" />
          </div>
          <button @click.prevent="addToCart" class="add-to-cart">
            DODAJ U KORPU
          </button>
        </div>
        <!-- </div> -->
      </div>
    </div>
    <h3><i class="fas fa-image"></i> {{ currentProduct.pro_title }}</h3>
    <div class="about-artwork">
      <p>{{ currentProduct.pro_description }}</p>
    </div>
    <h3>
      <i class="fas fa-bookmark"></i>
      {{ currentProduct.aut_fullName }}
    </h3>
    <div class="about-author">{{ currentProduct.aut_description }}</div>
    <div class="line"></div>
    <p class="numberOfComments">
      {{
        numberOfComments % 10 == 1 && numberOfComments % 100 !== 11
          ? numberOfComments + " Komentar"
          : numberOfComments + " Komentara"
      }}
    </p>
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
  width: 80%;
  margin: 0 auto;
  display: flex;
  flex-direction: column;
}
.image-order {
  margin: 30px 0 10px;
  padding: 10px;
  display: flex;
  justify-content: space-between;
  border-radius: 3px;
  border-bottom: 2px solid rgba(56, 21, 13, 0.1);
}
.bigImage {
  width: 60%;
}
.bigImage img {
  width: 100%;
}
.order {
  width: 35%;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}
.order h1,
.order p,
.order h2 {
  color: rgb(47, 85, 85);
  text-align: left;
}
.order h1 {
  font-size: 40px;
  padding: 0 0 10px;
}
.order h2 {
  font-size: 28px;
  font-weight: normal;
  padding: 0 0 30px;
}
.order p {
  font-family: "Roboto", sans-serif;
  font-size: 32px;
  font-weight: 500;
  padding: 0 0 5px;
}
.quantity input {
  padding: 5px;
  width: 30px;
  height: 25px;
  margin-right: 6px;
  background-color: rgba(56, 21, 13, 0.212);
  border: rgba(56, 21, 13, 0.068) 1px solid;
  text-align: center;
  font-size: 16px;
  outline: none;
}
.add-to-cart {
  color: rgb(255, 255, 255);
  background-color: rgba(56, 21, 13, 0.534);
  padding: 10px 0;
  border: none;
  box-shadow: 1px 1px 3px 0px rgba(0, 0, 0, 0.699);
  width: 250px;
  outline: none;
  text-align: none;
}
.add-to-cart:active {
  opacity: 0.85;
}
.add-to-cart:hover {
  background-color: rgb(23, 112, 112);
}
.qty-add {
  display: flex;
  flex-direction: row;
  justify-content: start;
  align-items: center;
  margin-bottom: 10px;
}
h3 {
  font-style: italic;
  color: rgb(65, 62, 63);
  text-align: left;
  font-size: 24px;
  margin: 20px 0;
  font-weight: 500;
}
.fa-bookmark,
.fa-image {
  color: rgba(12, 96, 102, 0.945);
  font-size: 40px;
}
.about-artwork,
.about-author {
  border-bottom: 2px solid rgba(56, 21, 13, 0.1);
  background-color: rgba(56, 21, 13, 0.1);
  border-radius: 5px;
  padding: 15px;
  text-align: left;
}
.line {
  border-bottom: 1px solid rgba(56, 21, 13, 0.534);
  margin-top: 10px;
}

.numberOfComments {
  font-style: italic;
  color: rgba(83, 80, 80, 0.856);
  text-align: right;
  padding: 10px;
}

@media screen and (max-width: 1110px) {
  .image-order {
    margin-top: 20px;
  }
  .wrapper {
    width: 95%;
  }
  .add-to-cart {
    width: 50%;
  }
}
@media screen and (max-width: 850px) {
  .order h1 {
    padding-top: 5px;
    font-size: 32px;
  }

  .image-order {
    margin-top: 10px;
    flex-direction: column;
  }
  .bigImage {
    width: 100%;
  }
  .order {
    width: 100%;
  }
  .add-to-cart {
    width: 220px;
  }
}
@media screen and (max-width: 590px) {
  .image-order {
    padding: 0 0;
  }
  .order h1 {
    font-size: 24px;
  }
  .order h2 {
    font-size: 18px;
  }
  .order p {
    font-size: 20px;
  }

  .about-artwork,
  .about-author {
    font-size: 12px;
  }
  .wrapper {
    width: 95%;
  }
  .add-to-cart {
    width: 180px;
  }
}
</style>
