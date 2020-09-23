<template>
  <div class="chat">
    <header class="header">
      <h1 class="title">FeatherJS</h1>
      <button class="btn" @click="logout">Logout</button>
    </header>
    <main class="main">
      <chat-messages />
    </main>
    <footer>
      <create-message />
    </footer>
  </div>
</template>

<script>
import CreateMessage from '../components/CreateMessage';
import ChatMessages from '../components/ChatMessages';
export default {
  methods: {
    logout() {
      this.$store.dispatch('auth/logout');
    },
  },
  components: {
    CreateMessage,
    ChatMessages,
  },
  mounted() {
    if (!this.isAuthenticated) {
      this.$router.replace({ name: 'Login' });
    }
  },
  computed: {
    isAuthenticated() {
      return this.$store.getters['auth/isAuthenticated'];
    },
  },
};
</script>

<style scoped>
header {
  padding: 1rem;
  box-shadow: 0 0.25rem 0.25rem rgba(0, 0, 0, 0.3);
  display: flex;
  justify-content: space-between;
  align-items: center;
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
}

.title {
  font-size: 2rem;
  color: blue;
}

.logo {
  height: 2rem;
  display: block;
}

main {
  height: calc(100vh - 150px);
  margin: 75px 0;
  overflow-y: scroll;
  padding: 2rem;
}

footer {
  position: fixed;
  bottom: 0;
  left: 0;
  width: 100%;
  border-top: solid 2px hsl(0, 6%, 77%);
}
</style>
