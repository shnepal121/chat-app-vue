<template>
  <div class="message-container">
    <chat-message
      v-for="message in messages"
      :key="message._id"
      :message="message"
    />
  </div>
</template>

<script>
import ChatMessage from './ChatMessage';

export default {
  name: 'MessageList',
  components: {
    ChatMessage,
  },
  created() {
    const { Message } = this.$FeathersVuex.api;
    Message.find({ query: {} });
  },
  computed: {
    messages() {
      return this.$store.getters['messages/list'];
    },
  },
  methods: {
    scrollToBottom() {
      this.$nextTick(() => {
        const node = document.querySelector('.main');
        node.scrollTop = node.scrollHeight;
      });
    },
  },
  watch: {
    messages() {
      this.messages.length && this.scrollToBottom();
    },
  },
};
</script>

<style scoped>
.message-container {
  max-width: 400px;
  margin: 0 auto;
}
</style>
