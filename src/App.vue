<template>
  <div id="app">
    <ChatBox></ChatBox>
  </div>
</template>

<script>
import ChatBox from './components/ChatBox.vue'

export default {
  name: 'App',
  components: {
    ChatBox,
  }
}
</script>

<style lang="scss">
:root {
  --light-gray: #F2F2F2;
  --medium-gray: #cbcbcb;
  --dark-gray: #2c3e50;
  --font-family:  Avenir, Helvetica, Arial, sans-serif;;
}

html, body {
  background: var(--light-gray);
  height: 100%;
}

#app {

  font-family: var(--font-family);
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: var(--dark-gray);
  height: 100%;
  display: flex;
}
</style>
