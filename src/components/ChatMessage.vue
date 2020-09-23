<template>
  <div class="message" :id="message._id">
    <img
      :src="message.user.avatar || placeholder"
      :alt="message.user.email"
      class="avatar"
    />
    <div class="message-wrapper">
      <p class="message-header">
        <span class="username">{{ message.user.email }}</span>
        <span class="sent-date">{{
          new Date(message.createdAt).toDateString()
        }}</span>
      </p>
      <p class="message-content">{{ message.text }}</p>
      <div class="message-actions">
        <app-dropdown :message="message" />
      </div>
    </div>
  </div>
</template>

<script>
import AppDropdown from '@/components/AppDropdown.vue';
export default {
  props: {
    message: {
      type: Object,
      required: true,
    },
  },
  components: {
    AppDropdown,
  },
  methods: {
    editMessage() {},
    deleteMessage() {},
  },
};
</script>

<style scoped>
.message {
  display: flex;
  position: relative;
}

.message:not(:last-child) {
  margin-bottom: 2rem;
}

.message-header {
  display: flex;
}

.sent-date {
  color: #777;
  margin-left: 0.25rem;
}

.username {
  color: blue;
}

img {
  display: block;
  width: 3rem;
  height: 3rem;
  object-fit: cover;
  border-radius: 50%;
  margin-right: 1rem;
}

.message-actions {
  position: absolute;
  top: 0;
  right: 0;
}
</style>
