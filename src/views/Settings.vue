<template>
  <div class="settings">
    <h2 class="subtitle">Settings</h2>
    <form v-on:submit.prevent="updatefire">
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

export default {
  name: "Settings",
  data() {
    return {
      name: "",
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
      .then((doc) => (this.name = doc.data().name));
  },
};
</script>
