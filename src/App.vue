<template>
  <div id="app">
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
              <button v-if="loggedIn" class="button is-light">
                Logout
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

    <section class="section">
      <div class="container">
        <router-view />
      </div>
    </section>
  </div>
</template>

<script>
import firebase from "firebase/app";
import "firebase/auth";

export default {
  data() {
    return {
      loggedIn: false,
      links: [
        { name: "Home", url: "/" },
        { name: "About", url: "/about" },
      ],
    };
  },
  beforeMount() {
    firebase
      .auth()
      .onAuthStateChanged((user) => user && (this.loggedIn = true));
  },
};
</script>

<style scoped>
.title {
  color: white;
  font-size: 1.5em;
}
</style>
