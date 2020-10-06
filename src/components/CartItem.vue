<template>
  <div class="container">
    <div class="imgtitle">
      <img :src="item.pro_image" class="image" />
      <div class="title">{{ item.pro_title }}</div>
    </div>
    <div class="qtyprice">
      <div class="price">
        <span> EUR {{ item.pro_price }},00 </span>
      </div>
      <div class="qty">
        <button @click="$emit('qty-decrement', item.pro_id)" class="qty-btn">
          <i class="close fa fa-minus"></i>
        </button>
        <span>{{ item.quantity }}</span>
        <button @click="$emit('qty-increment', item.pro_id)" class="qty-btn">
          <i class="close fa fa-plus"></i>
        </button>
      </div>
      <div class="total">EUR {{ item.pro_price * item.quantity }},00</div>
      <div class="delete">
        <button @click="$emit('delete-row', item.pro_id)" class="remove">
          <i class="fas fa-trash"></i>
        </button>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  props: ["item"],
};
</script>
<style scoped>
.container {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  border-top: rgba(56, 21, 13, 0.534) 1px dashed;
  margin: 5px auto;
  width: 90%;
  padding-top: 10px;
}
.imgtitle {
  display: flex;
  flex-direction: row;
  width: 45%;
}
.image {
  width: 40%;
}
.title {
  text-align: left;
  margin: auto 10px;
  font-size: 26px;
}
.qtyprice {
  display: flex;
  flex-direction: row;
}
.delete {
  margin: auto 10px;
}
.price {
  margin: auto 10px;
  font-size: 20px;
}
.qty {
  margin: auto 10px;
  font-size: 20px;
  /* min-width: 160px; */
}
.qty span {
  display: inline-block;
  min-width: 20px;
  text-align: center;
}
.qty-btn {
  background-color: rgba(56, 21, 13, 0.534);
  border: none;
  box-shadow: 2px 2px 3px 0px rgba(0, 0, 0, 0.699);
  outline: none;
  color: white;
  padding: 5px;
  width: 35px;
  margin: auto 10px;
  font-size: 15px;
}
.qty-btn:active {
  opacity: 0.85;
}
.qty-btn:hover {
  background-color: rgb(23, 112, 112);
}
.total {
  margin: auto 10px;
  font-size: 20px;
  min-width: 120px;
}
.remove {
  /* background-color: rgb(255, 0, 0); */
  background-color: rgba(231, 57, 18, 0.534);
  border: none;
  box-shadow: 3px 3px 3px 0px rgba(0, 0, 0, 0.699);
  outline: none;
  color: white;
  padding: 5px;
  width: 35px;
  font-size: 15px;
}
.remove:hover {
  background-color: rgb(255, 0, 0);
}
.remove:active {
  opacity: 0.85;
}

@media screen and (max-width: 1024px) {
  .title {
    font-size: 20px;
  }
  .price,
  .qty,
  .total {
    font-size: 18px;
  }
}
@media screen and (max-width: 950px) {
  .container {
    flex-wrap: wrap;
  }
  .imgtitle {
    width: 100%;
    margin: 0 0 20px 0;
  }
  .image {
    width: 65%;
  }
  .title {
    width: 35%;
    font-size: 30px;
    text-align: center;
  }
  .qtyprice {
    width: 100%;
    margin-bottom: 10px;
  }
  .price {
    width: 160px;
    text-align: left;
    margin: 0;
  }
  .qty {
    width: 40%;
    justify-content: left;
  }
  .total {
    width: 45%;
    text-align: right;
  }
}
@media screen and (max-width: 540px) {
  .container {
    width: 100%;
  }
  .imgtitle {
    width: 100%;
    margin-bottom: 20px;
    display: flex;
    flex-direction: column;
  }
  .image {
    width: 95%;
    margin: 0 auto;
  }
  .title {
    width: 93%;
    font-size: 28px;
    margin-top: 10px;
    text-align: left;
  }
  .qtyprice {
    flex-direction: column;
    margin-bottom: -40px;
  }
  .price {
    margin-left: 10px;
    font-size: 20px;
  }
  .qty {
    margin: 40px 0 20px;
    display: flex;
    flex-direction: row;
    z-index: 2;
  }
  .total {
    text-align: right;
    font-weight: bold;
    width: 73%;
    position: relative;
    bottom: 26%;
  }
  .delete {
    text-align: right;
    position: relative;
    bottom: 44%;
    right: 3%;
  }
}
</style>