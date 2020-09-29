<template>
  <div>
    <form @submit.prevent="checkout">
      <input type="text" v-model="address" placeholder="Address" class="forms" />
      <input type="text" v-model="country" placeholder="Country" class="forms" />
      <input type="text" v-model="city" placeholder="City" class="forms" />
      <input type="text" v-model="postcode" placeholder="Post Code" class="forms" />
      <button class="forms button">Finish</button>
    </form>
  </div>
</template>
<script>
import axios from "axios";
export default {
  data: function () {
    return {
      address: "",
      country: "",
      city: "",
      postcode: "",
    };
  },
  props: ["cart"],
  methods: {
    checkout() {
      axios
        .post("http://localhost:3000/orders", {
          ord_adress: this.address,
          ord_city: this.city,
          ord_country: this.country,
          ord_postcode: this.postcode,
          user_id: localStorage.getItem("user_id"),
          cart: this.cart,
        })
        .then((res) => {
          console.log(res.data);
        });
    },
  },
};
</script>
<style scoped>
.forms {
  font-family: FontAwesome, Arial, Helvetica, sans-serif;
  display: block;
  margin: 20px;
  padding: 5px;
  width: 250px;
  height: 25px;
  background-color: rgba(56, 21, 13, 0.274);
  border: rgba(56, 21, 13, 0.068) 1px solid;
  outline: none;
}
.button {
  width: 263px;
  height: 35px;
  margin-bottom: 30px;
  color: rgb(255, 255, 255);
  background-color: rgba(56, 21, 13, 0.534);
  padding: 5px;
  border: none;
  box-shadow: 3px 3px 3px 0px rgba(0, 0, 0, 0.699);
  outline: none;
}
.button:hover {
  background-color: rgb(23, 112, 112);
}
.error {
  color: red;
  font-size: 12px;
  width: 250px;
  margin: 0 0 10px 20px;
}
</style>
