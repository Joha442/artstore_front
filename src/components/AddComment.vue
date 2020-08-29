<template>
  <form @submit.prevent="addComment">
    <textarea type="text" v-model="comment" />
    <button type="submit">Post</button>
  </form>
</template>

<script>
import axios from "axios";
export default {
  data: function() {
    return {
      comment: "",
    };
  },
  props: ["pro_id"],

  methods: {
    addComment() {
      axios
        .post("http://localhost:3000/comments", {
          pro_id: this.pro_id, //will be provided as props from parent (ShowMore->Comments->AddComments) component
          user_id: localStorage.getItem("user_id"),
          com_content: this.comment,
        })
        .then((res) => {
          console.log(res.data);
          this.$emit("comment-added");
        });
    },
  },
};
</script>

<style scoped></style>
