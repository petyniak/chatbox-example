<template>
  <div class="comments-wrapper" ref="commentsWrapper">
    <div v-for="comment in comments" v-bind:key="comment.id" class="comment-container">
      <Avatar image-url="comment.avatar"/>
      <div class="comment">
        <strong>{{ comment.first_name }} {{ comment.last_name }}</strong> {{ comment.text }}
        <CommentTime :created-at="comment.created_at"/>
      </div>
    </div>
  </div>
</template>
<script>
import Avatar from "@/components/Avatar";
import CommentTime from "@/components/CommentTime";
import moment from 'moment'

moment.locale('pl');

export default {
  name: 'Comments',
  components: {CommentTime, Avatar},
  props: {
    comments: Array
  },
  watch: {
    comments: function () {
      // scroll down if new comment is added
      this.$nextTick(() => {
        this.$refs.commentsWrapper.scrollTop = this.$refs.commentsWrapper.scrollHeight;
      });
    }
  }
}
</script>

<style scoped lang="scss">
.comments-wrapper {
  flex: 2;
  overflow: auto;

  .comment-container {
    display: flex;
    padding: 1em 1em 1em 0;
    line-height: 1.4em;

    &:not(:first-child) {
      border-top: 1px solid #c8c8c8;
    }

  }
}
</style>

