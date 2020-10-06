<template>
  <div class="comment">
    <button
      v-if="loggedInUserId == comment.user_id || userLevel == 1"
      class="delete"
      @click="deleteComment(comment.com_id)"
    >
      <span><i class="fas fa-trash"></i></span>
      <span> OBRIŠI</span>
    </button>
    <button
      v-if="loggedInUserId == comment.user_id"
      class="edit"
      @click="isEdit = true"
    >
      <span><i class="fas fa-pencil-alt"></i></span>
      <span> IZMENI</span>
    </button>
    <div class="user">
      <p>
        <i class="fas fa-user"></i>
      </p>
      <div class="user-date">
        <p>{{ comment.user_username }}</p>
        <p>
          {{ comment.com_date }}
          <i class="far fa-clock"></i>
        </p>
      </div>
    </div>
    <p v-if="!isEdit" class="content">{{ comment.com_content }}</p>
    <div v-if="isEdit" class="edit-container">
      <textarea type="text" v-model="newText" class="newText" />
      <div class="edit-buttons">
        <button @click="confirm" :disabled="!newText" class="confirm">
          <span><i class="fas fa-check"></i></span>
          <span>POTVRDI</span>
        </button>
        <button @click="cancel" class="cancel">
          <span><i class="fas fa-times"></i></span>
          <span>OTKAŽI</span>
        </button>
      </div>
    </div>
  </div>
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
  created() {
    this.getUserLevel();
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
.user {
  display: flex;
  flex-direction: row;
  margin-bottom: 10px;
}
.fa-user {
  font-size: 50px;
  color: rgba(56, 21, 13, 0.534);
  margin-left: 12px;
}
.user-date {
  display: flex;
  flex-direction: column;
  margin-left: 10px;
}
.user-date p:nth-child(1) {
  font-weight: bold;
  color: black;
  font-size: 16px;
}
.user-date p:nth-child(2) {
  color: rgba(83, 80, 80, 0.856);
  font-size: 14px;
  margin-top: 2px;
}
.content {
  padding: 5px 10px 5px;
}
.delete,
.edit {
  color: rgb(255, 255, 255);
  background-color: rgba(56, 21, 13, 0.534);
  padding: 10px;
  border: none;
  box-shadow: 3px 3px 3px 0px rgba(0, 0, 0, 0.699);
  outline: none;
  width: 90px;
}
.delete {
  position: absolute;
  right: 10px;
  top: 7px;
  background-color: rgba(231, 57, 18, 0.534);
}
.edit {
  position: absolute;
  right: 108px;
  top: 7px;
}
.edit:hover {
  background-color: rgb(23, 112, 112);
  cursor: pointer;
}
.delete:hover {
  background-color: rgb(255, 0, 0);
  cursor: pointer;
}
.newText {
  width: 300px;
  min-height: 50px;
  resize: none;
  border: 1.2px solid rgba(56, 21, 13, 0.534);
  margin-bottom: 10px;
  padding: 5px;
}
.edit-container {
  display: flex;
  flex-direction: row;
}
.edit-buttons {
  height: 60px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  padding-left: 10px;
}
.edit-buttons span:nth-child(1) {
  display: none;
}
.confirm {
  width: 80px;
  color: rgb(255, 255, 255);
  background-color: rgba(56, 21, 13, 0.534);
  padding: 5px;
  border: none;
  box-shadow: 3px 3px 3px 0px rgba(0, 0, 0, 0.699);
  outline: none;
}
.cancel {
  width: 80px;
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

@media screen and (max-width: 1025px) {
  .delete span:nth-child(2),
  .edit span:nth-child(2) {
    display: none;
  }
  .delete,
  .edit {
    padding: 10px;
    width: 40px;
  }
  .edit {
    right: 55px;
  }
  .edit-buttons span:nth-child(1),
  .edit-buttons span:nth-child(1) {
    display: none;
  }
  .confirm span:nth-child(1),
  .cancel span:nth-child(1) {
    display: inline;
  }
  .confirm span:nth-child(2),
  .cancel span:nth-child(2) {
    display: none;
  }
  .confirm,
  .cancel {
    width: 40px;
  }
}
@media screen and (max-width: 400px) {
  .user-date p:nth-child(1) {
    font-weight: bold;
    font-size: 14px;
    color: black;
    width: 120px;
  }
  .user-date p:nth-child(2) {
    font-size: 12px;
  }
}
</style>
