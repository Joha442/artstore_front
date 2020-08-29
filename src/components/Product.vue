<template>
  <div class="container">
    <img :src="product.pro_image" />
    <p>{{ product.pro_title }}</p>
    <p>{{ product.aut_fullName }}</p>
    <p>{{ product.pro_description }}</p>

    <router-link :to="{ name: 'ShowMore', params: { pro_id: product.pro_id } }">Show More</router-link>
    <button>Add to Cart</button>
  </div>
</template>
<script>
export default {
  name: "Product",
  props: ["product"],
};
</script>
<style scoped>
.container {
  width: 360px;
  height: 420px;
  padding: 10px;
  margin: 5px 5px 10px 20px;
  transition: 1s;
}
.container:hover {
  transform: scale(1.2);
  z-index: 2;
}
</style>
