<template>
  <div>
    <AddComment @comment-added="commentAdded" :pro_id="pro_id" v-if="loggedInUserId" />
    <Comment
      v-for="comment in comments"
      :key="comment.com_id"
      :comment="comment"
      :loggedInUserId="loggedInUserId"
      @comment-deleted="commentDeleted"
    />
  </div>
</template>
<script>
import axios from "axios";
import AddComment from "./AddComment.vue";
import Comment from "./Comment.vue";

export default {
  components: {
    AddComment,
    Comment,
  },
  props: ["pro_id", "loggedInUserId"],
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
          this.comments.reverse();
          this.formatDates(this.comments);
        });
    },
    formatDates(oldComments) {
      var newComments = oldComments.map((comment) => {
        comment.com_date = this.formatDate(comment.com_date);
        return comment;
      });
      this.comments = newComments;
    },
    formatDate(oldDate) {
      var d = new Date(oldDate);
      var date =
        d.getDate().toString() +
        "." +
        d.getMonth().toString() +
        "." +
        d.getFullYear().toString();
      var h = d.getHours().toString();
      var m = d.getMinutes().toString();
      var time =
        (h.length < 2 ? "0" + h : h) + ":" + (m.length < 2 ? "0" + m : m);
      var fullDate = date + "\t" + time;
      return fullDate;
    },
    commentAdded() {
      this.getComments();
      this.$emit("comment-added");
    },
    commentDeleted() {
      this.getComments();
      this.$emit("comment-deleted");
    },
  },
};
</script>
<style scoped>
</style>
