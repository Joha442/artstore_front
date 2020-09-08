<template>
  <div>
    <AddComment @comment-added="getComments" :pro_id="pro_id" v-if="loggedInUserId" />
    <div v-for="comment in comments" :key="comment.com_id" class="comments">
      <p>
        <i class="fas fa-user"></i>
        <span class="user">{{comment.user_username}}</span>
      </p>
      <p class="date">
        {{comment.com_date}}
        <i class="far fa-clock"></i>
      </p>
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
      var h = d.getHours().toString();
      var m = d.getMinutes().toString();
      var time =
        (h.length < 2 ? "0" + h : h) + ":" + (m.length < 2 ? "0" + m : m);
      var fullDate = date + "\t" + time;
      return fullDate;
    },
  },
};
</script>
<style scoped>
.comments {
  text-align: left;
  margin-bottom: 20px;
  background-color: rgba(56, 21, 13, 0.1);
  padding: 10px;
  word-wrap: break-word;
}
.fa-user {
  font-size: 50px;
  color: rgba(56, 21, 13, 0.534);
}
.user {
  font-weight: bold;
  font-size: 16px;
  color: black;
  margin-left: 15px;
  position: relative;
  bottom: 25px;
}
.date {
  color: rgba(83, 80, 80, 0.856);
  margin-left: 60px;
  position: relative;
  bottom: 20px;
}
</style>
