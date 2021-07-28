<template>
  <ul class="mention-popover" v-if="users">
    <li v-for="(user,index) in users" v-bind:key="user.id" :class='{"active-item": currentItem === index}'
        :data-index=index
        @mouseover.self="handleMouseOver"
        @mousedown.prevent
        @click="handleClick"
    >
      <Avatar image-url="user.avatar"/>
      {{ user.first_name }} {{ user.last_name }}
    </li>
  </ul>
</template>
<script>
import Avatar from "@/components/Avatar";

export default {
  name: 'MentionsList',
  components: {Avatar},
  props: {
    users: Array,
  },
  data() {
    return {
      currentItem: 0
    }
  },
  methods: {
    nextMention() {
      if (this.currentItem < this.users.length - 1)
        this.currentItem++;
    },
    previousMention() {
      if (this.currentItem > 0)
        this.currentItem--;
    },
    selectMention() {
      this.$emit("mentionSelected", this.users[this.currentItem]);
      this.currentItem = 0;
    },
    handleClick(event) {
      this.currentItem = event.target.dataset.index;
      this.selectMention();
    },
    handleMouseOver(event) {
      this.currentItem = parseInt(event.target.dataset.index, 10);
    }
  }
}
</script>

<style lang="scss">
ul.mention-popover {

  list-style: none;
  background: var(--light-gray);
  font-weight: bold;
  padding: 0;
  margin: 0;
  -webkit-box-shadow: 0px -5px 5px 0px rgba(0, 0, 0, 0.1);
  box-shadow: 0px -5px 5px 0px rgba(0, 0, 0, 0.1);

  li {
    padding: .5em;
    border-top: 1px solid var(--medium-gray);
    display: flex;
    align-items: center;

    &.active-item {
      background: var(--medium-gray);
    }
  }
}
</style>