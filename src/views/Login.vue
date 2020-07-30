<template>
  <div class="login">
    <Notification
      v-if="error"
      v-on:close="error = false"
      type="is-danger"
      :message="errorMessage"
    />
    <form v-on:submit.prevent="login">
      <div class="field">
        <label class="label">Email</label>
        <div class="control">
          <input
            class="input"
            type="email"
            v-model="email"
            placeholder="e.g. alexsmith@gmail.com"
          />
        </div>
      </div>

      <div class="field">
        <label class="label">Password</label>
        <div class="control">
          <input
            class="input"
            v-model="password"
            type="password"
            placeholder="Password"
          />
        </div>
      </div>

      <div class="field">
        <div class="control">
          <button
            class="button is-primary"
            :class="loading && 'is-loading'"
            type="submit"
          >
            Login
          </button>
        </div>
      </div>
    </form>
  </div>
</template>

<script>
import firebase from "firebase/app";
import "firebase/auth";
import Notification from "../components/Notification";

export default {
  name: "Login",
  components: { Notification },
  data() {
    return {
      email: "",
      password: "",
      error: false,
      errorMessage: "",
      loading: false,
    };
  },
  methods: {
    login() {
      this.loading = true;
      firebase
        .auth()
        .signInWithEmailAndPassword(this.email, this.password)
        .then(
          () => this.$router.push("/"),
          (error) => {
            this.error = true;
            this.errorMessage = error.message;
            this.loading = false;
          }
        );
    },
  },
};
</script>
