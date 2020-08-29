<template>
  <div class="container">
    <img :src="product.pro_image" />
    <p>{{ product.pro_title }}</p>
    <p>{{ product.aut_fullName }}</p>
    <!-- <p>{{ product.pro_description }}</p> -->
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
  width: 346px;
  height: 360px;
  /* padding: 10px; */
  /* margin: 20px 0; */
  transition: 1s;
  border: 1px black dashed;
}
.container:hover {
  transform: scale(1.2);
  z-index: 2;
}
</style>
