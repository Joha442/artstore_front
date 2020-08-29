<template>
  <div>
    <p></p>
    <!-- More About Product, get by id from database -->
    <Comments :pro_id="pro_id" />
  </div>
</template>
<script>
import axios from "axios";
import Comments from "../components/Comments.vue";

export default {
  components: {
    Comments,
  },
  data: function() {
    return {
      pro_id: this.$route.params.pro_id,
    };
  },
};
</script>
<style scoped></style>
