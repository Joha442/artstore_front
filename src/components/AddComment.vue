<template>
  <form @submit.prevent="addComment">
    <textarea v-model="comment" class="addComment" placeholder="Unesite komentar"></textarea>
    <button :disabled="!comment.length" type="submit" class="post">POŠALJI</button>
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

<style scoped>
form {
  display: flex;
  flex-direction: row;
}
.addComment {
  width: 300px;
  min-height: 50px;
  resize: none;
  border: 1.2px solid rgba(56, 21, 13, 0.534);
  margin-bottom: 10px;
  padding: 5px;
}
.post {
  color: rgb(255, 255, 255);
  background-color: rgba(56, 21, 13, 0.534);
  padding: 5px;
  border: none;
  box-shadow: 3px 3px 3px 0px rgba(0, 0, 0, 0.699);
  height: 62px;
  margin-left: 5px;
  width: 80px;
}
.post:hover {
  background-color: rgb(23, 112, 112);
}
</style>
