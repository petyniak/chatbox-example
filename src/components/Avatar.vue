<template>
  <img :src="imageUrl" @error="loadFallBackImage" alt=""/>
</template>
<script>

export default {
  name: 'Avatar',
  props: {
    imageUrl: String
  },
  methods: {
    loadFallBackImage(event) {
      event.target.src = require('@/assets/avatar.png');
    }

  }
}
</script>

<style scoped lang="scss">

img {
  border-radius: 50%;
  width: 40px;
  height: 40px;
  padding: 0 1em;
}

</style>