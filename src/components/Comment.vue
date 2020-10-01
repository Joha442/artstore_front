<template>
  <!-- <div> -->
  <div class="comment">
    <button
      v-if="loggedInUserId==comment.user_id || userLevel==1"
      class="delete"
      @click="deleteComment(comment.com_id)"
    >OBRIŠI</button>
    <button v-if="loggedInUserId==comment.user_id" class="edit" @click="isEdit=true">IZMENI</button>
    <p>
      <i class="fas fa-user"></i>
      <span class="user">{{comment.user_username}}</span>
    </p>
    <p class="date">
      {{comment.com_date}}
      <i class="far fa-clock"></i>
    </p>
    <p v-if="!isEdit" class="content">{{ comment.com_content }}</p>
    <div v-if="isEdit">
      <textarea type="text" v-model="newText" class="newText" />
      <button @click="confirm" class="confirm">POTVRDI</button>
      <button @click="cancel" class="cancel">OTKAŽI</button>
    </div>
  </div>
  <!-- </div> -->
</template>
<script>
import axios from "axios";
export default {
  name: "Comment",
  props: ["comment", "loggedInUserId"],
  data: function () {
    return {
      userLevel: 0,
      isEdit: false,
      newText: this.comment.com_content,
    };
  },
  updated() {
    this.getUserLevel();
  },
  methods: {
    deleteComment(com_id) {
      axios
        .delete("http://localhost:3000/comments?com_id=" + com_id)
        .then((res) => {
          this.$emit("comment-deleted");
        });
    },
    getUserLevel() {
      this.loggedInUserId &&
        axios
          .get("http://localhost:3000/users?user_id=" + this.loggedInUserId)
          .then((res) => {
            this.userLevel = res.data.data[0].user_level;
          });
    },
    confirm() {
      this.comment.com_content = this.newText;
      axios.put(
        "http://localhost:3000/comments?com_id=" + this.comment.com_id,
        {
          com_content: this.newText,
        }
      );

      this.isEdit = false;
    },
    cancel() {
      this.isEdit = false;
      this.newText = this.comment.com_content;
    },
  },
};
</script>
<style scoped>
.comment {
  text-align: left;
  margin-bottom: 20px;
  background-color: rgba(56, 21, 13, 0.1);
  padding: 10px;
  word-wrap: break-word;
  position: relative;
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
.content {
  padding: 0 10px 5px;
}
.delete,
.edit {
  position: absolute;
  left: 92%;
  color: rgb(255, 255, 255);
  background-color: rgba(56, 21, 13, 0.534);
  padding: 10px;
  border: none;
  box-shadow: 3px 3px 3px 0px rgba(0, 0, 0, 0.699);
  outline: none;
}
.edit {
  position: absolute;
  left: 85%;
}
.delete:hover,
.edit:hover {
  background-color: rgb(23, 112, 112);
}
.newText {
  width: 300px;
  min-height: 50px;
  resize: none;
  border: 1.2px solid rgba(56, 21, 13, 0.534);
  margin-bottom: 10px;
  padding: 5px;
}
.confirm {
  width: 80px;
  position: relative;
  bottom: 55px;
  left: 5px;
  color: rgb(255, 255, 255);
  background-color: rgba(56, 21, 13, 0.534);
  padding: 5px;
  border: none;
  box-shadow: 3px 3px 3px 0px rgba(0, 0, 0, 0.699);
  outline: none;
}
.cancel {
  width: 80px;
  position: relative;
  bottom: 20px;
  right: 75px;
  color: rgb(255, 255, 255);
  background-color: rgba(56, 21, 13, 0.534);
  padding: 5px;
  border: none;
  box-shadow: 3px 3px 3px 0px rgba(0, 0, 0, 0.699);
  outline: none;
}
.confirm:hover,
.cancel:hover {
  background-color: rgb(23, 112, 112);
}
</style>
