<template>
  <div class="dropdown">
    <div
      class="dropdown__btn-container "
      v-if="user._id === message.userId"
      @click="isOpen = !isOpen"
    >
      <button
        class="dropdown__btn"
        :class="{ 'dropdown__btn--active': isOpen }"
      ></button>
    </div>
    <button
      v-if="isOpen || willEdit"
      tabindex="-1"
      class="dropdown__backdrop"
      @click="isOpen = false"
    ></button>
    <div class="dropdown__content" v-if="isOpen">
      <a class="dropdown__link" @click="editMessage" href="#">Edit</a>
      <a class="dropdown__link" @click="deleteMessage" href="#">Delete</a>
    </div>
    <div class="edit-form" v-if="willEdit">
      <form class="edit">
        <input
          type="text"
          :value="messageText"
          @input="updatedText = $event.target.value"
        />
        <div>
          <button class="btn" @click.prevent="updateMessage">Save</button
          >&nbsp;<button @click.prevent="closeEditModel" class="btn">
            Cancel
          </button>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    message: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      isOpen: false,
      willEdit: false,
      updatedText: '',
    };
  },
  methods: {
    handleEscape(e) {
      if (e.key === 'Esc' || e.key === 'Escape') {
        this.isOpen = false;
      }
    },
    editMessage() {
      this.willEdit = true;
    },
    deleteMessage() {
      this.$store.dispatch('messages/remove', this.message._id);
      this.isOpen = false;
    },
    updateMessage() {
      const data = {
        text: this.updatedText,
        createdAt: new Date(),
        userId: this.message.userId,
      };
      this.$store.dispatch('messages/update', [this.message._id, data, {}]);
      this.closeEditModel();
    },
    closeEditModel() {
      this.willEdit = false;
      this.isOpen = false;
    },
  },
  computed: {
    messageText() {
      return this.message.text;
    },
    user() {
      return this.$store.getters['auth/user'];
    },
  },
  created() {
    document.addEventListener('keydown', this.handleEscape);
  },
  destroyed() {
    document.removeEventListener('keydown', this.handleEscape);
  },
};
</script>

<style scoped>
.dropdown__btn-container {
  cursor: pointer;
  width: 1.5rem;
  height: 1.5rem;
  border-radius: 50%;
  background-color: #f9f9f9;
  display: flex;
  align-items: center;
  justify-content: center;
}

.dropdown__btn {
  border: none;
  width: 0.25rem;
  height: 0.25rem;
  border-radius: 50%;
  background-color: #777;
  position: relative;
  outline: none;
}

.dropdown__btn::before,
.dropdown__btn::after {
  content: '';
  display: inline-block;
  width: 0.25rem;
  height: 0.25rem;
  border-radius: 50%;
  background-color: #777;
  position: absolute;
  right: 0;
}

.dropdown__btn::before {
  top: -0.3rem;
}

.dropdown__btn::after {
  top: 0.3rem;
}

.dropdown__btn--active {
  z-index: 2000;
}

.dropdown__icon {
  width: 2rem;
  height: 2rem;
  line-height: 0;
  fill: var(--color-primary);
  margin-top: 0.5rem;
}

.dropdown__backdrop {
  width: 100%;
  height: 100vh;
  position: fixed;
  top: 0;
  left: 0;
  background-color: rgba(0, 0, 0, 0.3);
  z-index: 1500;
}

.dropdown__content {
  z-index: 2000;
  position: absolute;
  top: -1rem;
  left: 1.5rem;
  text-align: center;
  display: inline-block;
  border-radius: 3px;
  padding: 0.25rem;
  background: #fff;
  box-shadow: 4px 4px 0.5rem rgba(0, 0, 0, 0.7);
}

.dropdown__link:link,
.dropdown__link:visited {
  display: block;
  text-decoration: none;
  color: #888;
  font-weight: 400;
  font-size: 13px;
  padding: 3px;
  background-color: transparent;
  transition: all 0.3s;
  margin: 0.25rem 0;
}

.dropdown__link:not(:last-child) {
  border-bottom: solid 1px #eee;
}

.dropdown__link:hover {
  background-color: blue;
  color: #fff;
}

.edit-form {
  position: fixed;
  width: 500px;
  top: 40%;
  left: 50%;
  z-index: 3000;
  transform: translate(-50%, -50%);
  padding: 2rem;
  background: white;
  border-radius: 3px;
}

input {
  width: 100%;
  padding: 0.75rem 1rem;
  margin-bottom: 0.5rem;
  display: inline-block;
}
</style>
