<template>
  <nav class="navbar is-dark" role="navigation" aria-label="main navigation">
    <div class="navbar-brand">
      <router-link class="navbar-item" to="/">
        <h1 class="title">Gym Buddy</h1>
      </router-link>

      <a
        role="button"
        class="navbar-burger burger"
        aria-label="menu"
        aria-expanded="false"
        data-target="navbarBasicExample"
      >
        <span aria-hidden="true"></span>
        <span aria-hidden="true"></span>
        <span aria-hidden="true"></span>
      </a>
    </div>

    <div id="navbarBasicExample" class="navbar-menu">
      <div class="navbar-start" v-if="loggedIn">
        <router-link
          v-for="link in links"
          :key="link.name"
          :to="link.url"
          class="navbar-item"
        >
          {{ link.name }}
        </router-link>
      </div>

      <div class="navbar-end">
        <div class="navbar-item">
          <div class="buttons">
            <button
              v-if="loggedIn"
              v-on:click="logout()"
              class="button is-light"
            >
              Logout ({{ this.email }})
            </button>
            <span v-else>
              <router-link to="/register" class="button is-primary">
                Register
              </router-link>
              <router-link to="/login" class="button is-light">
                Login
              </router-link>
            </span>
          </div>
        </div>
      </div>
    </div>
  </nav>
</template>

<script>
import firebase from "firebase/app";
import "firebase/auth";

export default {
  data() {
    return {
      loggedIn: false,
      email: "",
      links: [
        { name: "Home", url: "/home" },
        { name: "Add Workout", url: "/add" },
        { name: "Settings", url: "/settings" },
      ],
    };
  },
  methods: {
    logout() {
      this.loggedIn = false;
      firebase.auth().signOut();
    },
  },
  beforeMount() {
    firebase.auth().onAuthStateChanged((user) => {
      if (user) {
        this.loggedIn = true;
        this.email = firebase.auth().currentUser.email;
      }
    });
  },
};
</script>

<style scoped>
.title {
  color: white;
  font-size: 1.5em;
}
</style>
