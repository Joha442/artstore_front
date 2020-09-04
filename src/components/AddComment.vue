<template>
  <form @submit.prevent="addComment">
    <input type="text" v-model="comment" />
    <button :disabled="!comment.length" type="submit">Post</button>
  </form>
</template>

<script>
import axios from "axios";
export default {
  data: function () {
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
          this.$emit("comment-added");
          this.comment = "";
        });
    },
  },
};
</script>

<style scoped></style>
