<template>
  <form v-if="!isAuthenticated">
    <div class="error form__group" v-if="error">
      <p>{{ error.message }}</p>
      <a href="#" @click.prevent="dismissError">&times;</a>
    </div>
    <div class="form__group">
      <label for="email">Email address</label>
      <input type="email" v-model="form.email" id="email" required="true" />
    </div>
    <div class="form__group">
      <label for="password">Password</label>
      <input
        type="password"
        v-model="form.password"
        id="password"
        required="true"
      />
    </div>
    <button class="btn" @click.prevent="login">Login</button>&nbsp;
    <button class="btn" @click.prevent="signUp">SignUp &amp; login</button>
  </form>
</template>

<script>
export default {
  data() {
    return {
      error: '',
      form: {
        email: '',
        password: '',
      },
    };
  },
  mounted() {
    if (this.isAuthenticated) {
      this.$router.replace({ name: 'Chat' });
    }
  },
  methods: {
    login() {
      this.dismissError();

      this.$store
        .dispatch('auth/authenticate', {
          strategy: 'local',
          email: this.form.email,
          password: this.form.password,
        })
        // Just use the returned error instead of mapping it from the store.
        .catch((err) => {
          console.log(err);
          // Convert the error to a plain object and add a message.
          let type = err.className;
          err = Object.assign({}, err);
          err.message =
            type === 'not-authenticated'
              ? 'Incorrect email or password.'
              : 'An error prevented login.';
          this.error = null;
        });
    },
    signUp() {
      this.dismissError();

      const { User } = this.$FeathersVuex.api;
      // Automatically log the user in after successful signup.
      new User(this.form)
        .save()
        .then(() =>
          this.$store.dispatch('auth/authenticate', {
            strategy: 'local',
            email: this.form.email,
            password: this.form.password,
          })
        )
        // Just use the returned error instead of mapping it from the store.
        .catch((err) => {
          // Convert the error to a plain object and add a message.
          let type = err.errorType;
          err = Object.assign({}, err);
          err.message =
            type === 'uniqueViolated'
              ? 'That email address is unavailable.'
              : 'An error prevented signup.';
          this.error = err;
        });
    },
    dismissError() {
      this.error = null;
    },
  },
  computed: {
    isAuthenticated() {
      return this.$store.getters['auth/isAuthenticated'];
    },
  },
};
</script>

<style scoped>
form {
  width: 400px;
  margin: 4rem auto;
  padding: 2rem;
  box-shadow: 0 0.5rem 0.5rem rgba(0, 0, 0, 0.3);
}

label,
input {
  display: block;
}

.form__group {
  margin-bottom: 2rem;
}

input {
  width: 80%;
  padding: 0.75rem 1rem;
}

label {
  margin-bottom: 0.25rem;
}

.error {
  display: flex;
  background: #f7f7f7;
  justify-content: space-between;
  align-items: center;
  padding: 0.5rem;
}

.error p {
  color: red;
}

.error a {
  text-decoration: none;
  color: #000;
  line-height: 1;
  transform: scale(1.5);
}
</style>
