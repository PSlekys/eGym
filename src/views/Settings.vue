<template>
  <div class="settings">
    <h2 class="subtitle">Settings</h2>
    <Notification :display="error" :type="errorType" :message="errorMessage" />
    <form v-on:submit.prevent="update">
      <div class="field">
        <label class="label" for="name">Name</label>
        <div class="control">
          <input
            class="input"
            type="text"
            v-model="name"
            placeholder="Petras Slekys"
          />
        </div>
      </div>

      <div class="field">
        <div class="control">
          <button class="button is-primary" type="submit">Update</button>
        </div>
      </div>
    </form>
  </div>
</template>

<script>
import firebase from "firebase/app";
import "firebase/firestore";
import "firebase/auth";
import Notification from "../components/Notification";

export default {
  name: "Settings",
  components: { Notification },
  data() {
    return {
      name: "",
      error: false,
      errorType: "",
      errorMessage: "",
    };
  },
  methods: {
    update() {
      firebase
        .firestore()
        .collection("users")
        .doc(firebase.auth().currentUser.uid)
        .update({
          name: this.name,
        });
    },
  },
  beforeMount() {
    firebase
      .firestore()
      .collection("users")
      .doc(firebase.auth().currentUser.uid)
      .get()
      .then(
        (doc) => (this.name = doc.data().name),
        (error) => {
          this.error = true;
          this.errorType = "is-danger";
          this.errorMessage = error.message;
        }
      );
  },
};
</script>
