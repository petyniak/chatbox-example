<template>
  <div class="textarea-wrapper">
    <MentionsList :users="filteredMentionUsers" ref="mentionsList" @mentionSelected="handleMentionSelected"/>
    <div class="textarea-container">
      <Avatar image-url="client.avatar"/>
      <textarea class="textarea" tabindex="0"
                placeholder="Wpisz treść wiadomości"
                ref="textarea"
                v-model="commentContent"
                @keydown.enter.prevent
                @keydown="handleKeyDown"
                @input="checkForMentionPattern"
                @focusout="hideMentionPopOver"
      >
      </textarea>
      <a href="" class="button" @click.prevent="submit">Opublikuj</a>
    </div>
  </div>
</template>
<script>

import Avatar from "@/components/Avatar";
import MentionsList from "@/components/MentionsList";

export default {
  name: 'InputBox',
  components: {MentionsList, Avatar},
  props: {
    users: Array,
    client: Object,
  },

  data() {
    return {
      isMentionPopoverVisible: false,
      filteredMentionUsers: [],
      previousMentionPatternMatches: [],
      currentMentionPatternMatch: '',
      commentContent: ''
    }
  },

  methods: {
    submit() {
      if (this.commentContent.length) {
        this.$emit("addNewComment", {text: this.commentContent, mentions: []});
        this.commentContent = '';
      }
    },

    handleKeyDown(event) {
      if (this.isMentionPopoverVisible && ['ArrowDown', 'ArrowUp', 'Enter'].includes(event.key)) {
        event.preventDefault();

        if (event.key === 'ArrowDown')
          this.$refs.mentionsList.nextMention();

        if (event.key === 'ArrowUp')
          this.$refs.mentionsList.previousMention();

        if (event.key === 'Enter')
          this.$refs.mentionsList.selectMention();

      } else {
        if (event.key === 'Enter') {
          event.preventDefault();
          this.submit()
        }
      }
    },

    checkForMentionPattern() {
      // look for mention pattern in input content
      const regex = /(^| )@[\w]+/gm;
      let matches = this.commentContent.match(regex);
      this.currentMentionPatternMatch = '';
      if (matches !== null) {
        // filter all matches to find current one not old or uncompleted
        this.currentMentionPatternMatch = matches.filter(x => !this.previousMentionPatternMatches.includes(x));
        // got new one, start looking for users to display available mentions
        this.previousMentionPatternMatches = matches;
      } else {
        // if no matches clear variable to hide available mentions
        this.previousMentionPatternMatches = [];
      }
    },

    searchForMentionUsers: function () {
      this.filteredMentionUsers = [];
      if (this.currentMentionPatternMatch.length > 0) {
        let mention = this.currentMentionPatternMatch.toString().trim();
        mention = mention.replace(/[\s@]/g, '');

        this.filteredMentionUsers = this.users.filter(user => {
          return user.first_name.toLowerCase().includes(mention) || user.last_name.toLowerCase().includes(mention);
        });
      }
    },
    handleMentionSelected: function (eventData) {
      if (eventData) {
        let fullName = eventData.first_name + ' ' + eventData.last_name;
        let mention = this.currentMentionPatternMatch.toString().trim();
        let cursorPositionAtBeginningOfMention = this.$refs.textarea.selectionStart - mention.length;

        // insert name instead of mention
        this.commentContent = this.commentContent.replace(mention, fullName);

        //move cursov at the end of insertion
        this.$nextTick(() => {
          this.$refs.textarea.selectionEnd = this.$refs.textarea.selectionEnd = cursorPositionAtBeginningOfMention + fullName.length;
        });
        this.hideMentionPopOver();
      }
    },

    hideMentionPopOver: function () {
      this.filteredMentionUsers = [];
    }
  },

  watch: {
    currentMentionPatternMatch: function () {
      this.searchForMentionUsers()
    },
    filteredMentionUsers: function () {
      this.isMentionPopoverVisible = !!this.filteredMentionUsers.length;
    }
  }
}
</script>

<style scoped lang="scss">
.textarea-wrapper {
  position: relative;
}

.textarea-container {
  display: flex;
  padding: 1rem 0;
  background: #fff;
  border-top: 1px solid var(--medium-gray);

  .textarea {
    min-height: 50px;
    padding: .5rem;
    flex-grow: 1;
    border: none;
    font-family: var(--font-family);
  }

  .button {
    font-weight: bold;
    color: var(--dark-gray);
    height: 50%;
    font-size: 0.75rem;
    align-self: center;
    padding: 10px;
    text-decoration: none;

    &:hover {
      color: aqua;
    }
  }
}
</style>