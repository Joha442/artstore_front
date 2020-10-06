<template>
  <div>
    <div v-if="!isSuccessOrder">
      <input
        type="text"
        v-model="address"
        placeholder="Adresa*"
        class="forms top"
      />

      <input type="text" v-model="country" placeholder="Zemlja" class="forms" />
      <input type="text" v-model="city" placeholder="Grad" class="forms" />
      <input
        type="text"
        v-model="postcode"
        placeholder="Poštanski kod"
        class="forms"
      />
      <p class="error">{{ error }}</p>
      <button class="button" @click.prevent="checkout">POTVRDI</button>
      <p class="adress">* Neophodna polja</p>
    </div>
    <div v-if="isSuccessOrder">
      <p class="message">{{ message }}</p>
      <button class="successBtn" @click="closeForm">ZATVORI</button>
    </div>
  </div>
</template>
<script>
import axios from "axios";
import router from "../router";
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
    checkout() {
      if (!this.address.length) {
        this.error = "Unesite adresu";
        return;
      } else if (!/^[0-9]+$/.test(this.postcode)) {
        this.error = "Neispravan poštanski broj";
      } else {
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
            console.log(res);
            if (res.data.result == "OK") {
              this.message = "Uspešno ste poručili proizvode";
              this.isSuccessOrder = true;
              this.$emit("clear-cart");
            }
          });
      }
    },
    closeForm() {
      this.$emit("close-modal");
      router.push({ path: "/" });
    },
  },
};
</script>
<style scoped>
.forms {
  font-family: FontAwesome, Arial, Helvetica, sans-serif;
  display: block;
  margin: 20px auto;
  padding: 5px;
  width: 80%;
  height: 25px;
  background-color: rgba(56, 21, 13, 0.274);
  border: rgba(56, 21, 13, 0.068) 1px solid;
  outline: none;
}
.button {
  width: 84%;
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
  margin: 10px 25px;
}
.top {
  margin-top: 50px;
}
.message {
  margin: 30px 0 20px;
  text-align: center;
  font-size: 18px;
}
.successBtn {
  width: 80px;
  color: rgb(255, 255, 255);
  background-color: rgba(56, 21, 13, 0.534);
  padding: 5px;
  border: none;
  box-shadow: 3px 3px 3px 0px rgba(0, 0, 0, 0.699);
  outline: none;
  margin-bottom: 20px;
}
.successBtn:active {
  opacity: 0.85;
}
.successBtn:hover {
  background-color: rgb(23, 112, 112);
}
</style>
