<template>
  <form @submit.prevent="checkout">
    <input type="text" v-model="address" placeholder="Address" />
    <input type="text" v-model="country" placeholder="Country" />
    <input type="text" v-model="city" placeholder="City" />
    <input type="text" v-model="postcode" placeholder="Post Code" />
    <button>Finish</button>
  </form>
</template>
<script>
import axios from "axios";
export default {
  data: function() {
    return {
      address: "",
      country: "",
      city: "",
      postcode: "",
    };
  },
  methods: {
    checkout() {
      axios
        .post("http://localhost:3000/orders", {
          ord_adress: this.address,
          ord_city: this.city,
          ord_country: this.country,
          ord_postcode: this.postcode,
          user_id: localStorage.getItem("user_id"),
        })
        .then((res) => {
          console.log(res.data);
        });
    },
  },
};
</script>
<style scoped></style>
