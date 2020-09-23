<template>
  <div id="app">
    <router-view></router-view>
  </div>
</template>

<script>
export default {
  name: 'App',
  mounted() {
    this.$store.dispatch('auth/authenticate').catch((error) => {
      if (!error.message.includes('Could not find stored JWT')) {
        console.error(error);
      }
    });
  },
  computed: {
    isAuthenticated() {
      return this.$store.getters['auth/isAuthenticated'];
    },
  },
  watch: {
    isAuthenticated(value) {
      const toRouteName = value ? 'Chat' : 'Login';
      if (this.$route.name !== toRouteName)
        this.$router.replace({ name: toRouteName });
    },
  },
};
</script>

<style>
*,
*::before,
*::after {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: sans-serif;
}

.btn {
  border: none;
  background: blue;
  color: #fff;
  font: inherit;
  padding: 0.5rem 1rem;
  border-radius: 0.5rem;
  cursor: pointer;
}
</style>
