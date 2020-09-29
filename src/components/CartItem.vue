<template>
  <div class="container">
    <img :src="item.pro_image" class="image" />
    <div class="title">{{item.pro_title}}</div>
    <div class="price">
      {{item.pro_price}}
      <i class="fas fa-euro-sign"></i>
    </div>
    <div class="qty">
      <!-- <button @click="$emit('qty-decrement', item.pro_id)" class="qty-btn"> -->
      <button @click="$emit('qty-decrement', item.pro_id)" class="qty-btn">
        <i class="close fa fa-minus"></i>
      </button>
      <span>{{item.quantity}}</span>
      <button @click="$emit('qty-increment',item.pro_id)" class="qty-btn">
        <i class="close fa fa-plus"></i>
      </button>
    </div>

    <div class="total">
      {{item.pro_price*item.quantity}}
      <i class="fas fa-euro-sign"></i>
    </div>
    <div class="delete">
      <button @click="$emit('delete-row', item.pro_id)" class="remove">X</button>
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
  padding: 10px;
  border-top: rgba(56, 21, 13, 0.534) 1px solid;
  margin: 0 auto;
  width: 900px;
}

.image {
  width: 150px;
}
.title {
  padding: 40px 10px;
  width: 300px;
  text-align: left;
}
.price {
  padding: 40px 10px;
  width: 50px;
  margin-left: 100px;
}
.qty {
  padding: 25px 5px;
  width: 150px;
}
.qty-btn {
  background-color: rgba(56, 21, 13, 0.534);
  /* border-color: 5px solid rgba(0, 0, 0, 0.918); */
  border: none;
  box-shadow: 3px 3px 3px 0px rgba(0, 0, 0, 0.699);
  outline: none;
  color: white;
  padding: 5px;
  width: 30px;
  margin: 10px;
}
.qty-btn:active {
  opacity: 0.85;
}
.qty-btn:hover {
  background-color: rgb(23, 112, 112);
}
.total {
  padding: 40px 5px;
  width: 60px;
  margin-left: -20px;
}
.remove {
  background-color: rgba(231, 57, 18, 0.534);
  border: none;
  box-shadow: 3px 3px 3px 0px rgba(0, 0, 0, 0.699);
  outline: none;
  color: white;
  padding: 5px;
  width: 30px;
}
.remove:active {
  margin-top: -2px;
  opacity: 0.85;
}
.delete {
  padding: 35px 5px;
}
.remove:hover {
  background-color: red;
}
</style>