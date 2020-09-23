<template>
  <form class="form" @submit.prevent="onSubmit">
    <input v-model="message.text" type="text" class="flex flex-1" />

    <button class="btn" type="submit">
      Send
    </button>
  </form>
</template>

<script>
export default {
  name: 'CreateMessage',
  data() {
    return {
      message: {
        text: '',
      },
    };
  },
  methods: {
    onSubmit() {
      const { Message } = this.$FeathersVuex.api;

      if (!this.message.text) {
        return;
      }
      const message = new Message(this.message);
      message.save();

      this.message.text = '';
    },
  },
};
</script>

<style scoped>
.form {
  width: 100%;
  padding: 1rem;
  display: flex;
  justify-content: space-between;
}

input {
  width: 90%;
  display: block;
  padding: 0.75rem 1rem;
}
</style>
