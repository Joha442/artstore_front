<template>
  <div>
    <div v-if="!isSuccessOrder">
      <input type="text" v-model="address" placeholder="Adresa*" class="forms" />
      <p class="error">{{error}}</p>
      <input type="text" v-model="country" placeholder="Zemlja" class="forms" />
      <input type="text" v-model="city" placeholder="Grad" class="forms" />
      <input type="text" v-model="postcode" placeholder="Poštanski kod" class="forms" />
      <button class="button" @click.prevent="checkout">
        <i class="far fa-check-square"></i> POTVRDI
      </button>
      <p class="adress">* Neophodna polja</p>
    </div>
    <div v-if="isSuccessOrder">
      <p>{{message}}</p>
      <button>ZATVORI</button>
    </div>
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
      error: "",
      isSuccessOrder: false,
      message: "",
    };
  },
  props: ["cart"],
  methods: {
    // successOrder() {
    //   this.isSuccessOrder = true;
    // },
    checkout() {
      if (!this.address.length) {
        this.error = "Unesite adresu";
        return;
      }
      if (this.address.length) {
        this.error = "";
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
            if (res.data.result == "OK")
              this.message = "USPEŠNO STE PORUCILI PROIZVODE";
            this.isSuccessOrder = true;
          });
      }
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
  margin-bottom: 10px;
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
.adress {
  font-size: 12px;
  text-align: left;
  margin: 10px 20px;
}
</style>
