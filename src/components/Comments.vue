<template>
  <div>
    <AddComment @comment-added="getComments" :pro_id="pro_id" />
    <div v-for="comment in comments" :key="comment.com_id">
      <p>{{ comment.com_content }}</p>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import AddComment from "./AddComment.vue";

export default {
  components: {
    AddComment,
  },
  props: ["pro_id"],
  data: function () {
    return {
      comments: [],
    };
  },
  created: function () {
    this.getComments();
  },
  methods: {
    getComments() {
      axios
        .get("http://localhost:3000/comments?pro_id=" + this.pro_id)
        .then((res) => {
          this.comments = res.data.data;
        });
    },
  },
};
</script>
