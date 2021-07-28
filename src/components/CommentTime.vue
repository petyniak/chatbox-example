<template>
  <div class="time">{{ relativeTime(createdAt) }}</div>
</template>

<script>
import moment from 'moment'

moment.locale('pl');
export default {
  name: 'CommentTime',
  props: {
    createdAt: String
  },
  data() {
    return {
      timer: {}
    }
  },
  created() {
    // auto update relative time every minute
    this.timer = setInterval(this.refreshFromNowTime, 60 * 1000);
  },
  methods: {
    relativeTime(time) {
      return moment(time, "YYYY-MM-DD hh:mm:ss").fromNow();
    },
    refreshFromNowTime() {
      this.$forceUpdate();
    }
  },
  beforeDestroy() {
    clearInterval(this.timer);
  }
}
</script>

<style scoped lang="scss">
.time {
  margin-top: 1em;
  font-size: 0.75em;
  color: gray;
}
</style>