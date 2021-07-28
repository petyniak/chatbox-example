<template>
  <div class="chat-box-container">
    <CommentsList :comments="comments"></CommentsList>
    <InputBox @addNewComment="handleAddNewComment" :users="users" :client="client"></InputBox>
  </div>
</template>

<script>
import InputBox from "@/components/InputBox";
import CommentsList from "@/components/CommentsList";
import axios from "axios";
import moment from "moment";

export default {
  name: 'ChatBox',
  components: {CommentsList, InputBox},
  props: {
    msg: String
  },
  data() {
    return {
      comments: [],
      users: [],
      client: {},
    }
  },
  async created() {
    // GET USERS
    axios.get('https://reqres.in/api/users')
        .then((res) => {
          this.users = res.data.data;
          this.client = this.users.shift();
        }).catch((err) => {
      console.error(err);
    });

    //GET INITIAL COMMENTS
    axios.get('./comments.json')
        .then((res) => {
          this.comments = res.data.data;
        }).catch((err) => {
      console.error(err);
    });
  },
  methods: {
    handleAddNewComment(eventData) {
      this.comments.push({
        "id": Date.now(),
        "text": eventData.text,
        "first_name": this.client.first_name,
        "last_name": this.client.last_name,
        "created_at": moment().format("YYYY-MM-DD HH:mm:ss")
      });
    },

  }
}
</script>

<style lang="scss">
.chat-box-container {
  max-width: 400px;
  text-align: left;
  background: #fff;
  margin-left: auto;
  align-self: flex-end;
  position: relative;
  display: flex;
  flex-flow: column;
  height: 500px;
  -webkit-box-shadow: 0px 0px 15px 0px rgba(0,0,0,0.5);
  box-shadow: 0px 0px 15px 0px rgba(0,0,0,0.5);
}
</style>
