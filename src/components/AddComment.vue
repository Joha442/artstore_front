<template>
  <div>
    <textarea
      v-model="comment"
      class="addComment"
      placeholder="Unesite komentar"
    ></textarea>
    <button
      :disabled="!comment.length"
      @click="addComment"
      class="post-comment"
    >
      POŠALJI
    </button>
  </div>
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
div {
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
.post-comment {
  color: rgb(255, 255, 255);
  background-color: rgba(56, 21, 13, 0.534);
  padding: 5px;
  border: none;
  box-shadow: 3px 3px 3px 0px rgba(0, 0, 0, 0.699);
  height: 62px;
  margin-left: 5px;
  width: 80px;
  outline: none;
}
.post-comment:focus:enabled {
  margin-top: -2px;
}
.post-comment:hover:enabled {
  background-color: rgb(23, 112, 112);
  cursor: pointer;
}
.post-comment:disabled {
  cursor: not-allowed;
  opacity: 0.9;
  box-shadow: none;
}

@media screen and (max-width: 500px) {
  .addComment {
    width: 80%;
  }
  .post-comment {
    margin-left: 3px;
    width: 25%;
  }
}
</style>
