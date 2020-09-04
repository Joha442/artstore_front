<template>
  <div>
    <AddComment @comment-added="getComments" :pro_id="pro_id" v-if="loggedInUserId" />
    <div v-for="comment in comments" :key="comment.com_id">
      <p>{{ comment.com_content }}</p>
      <p>{{comment.com_date}}</p>
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
          this.formatDates(this.comments);
          console.log(this.comments);
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
        "/" +
        d.getMonth().toString() +
        "/" +
        d.getFullYear().toString();
      var time =
        (d.getHours().toString().length < 2
          ? "0" + d.getHours().toString()
          : d.getHours().toString()) +
        ":" +
        d.getMinutes().toString();
      var fullDate = date + "  " + time;
      return fullDate;
    },
  },
};
</script>
<style scoped>
</style>
