<template>
  <div class="settings">
    <h2 class="subtitle">Settings</h2>
    <Notification :display="error" :type="errorType" :message="errorMessage" />
    <form v-on:submit.prevent="update">
      <div class="field-body">
        <div class="field">
          <label class="label">Name</label>
          <div class="control">
            <input
              class="input"
              type="text"
              placeholder="Name"
              v-model="firstname"
              required
            />
          </div>
        </div>
        <div class="field">
          <label class="label">Surname</label>
          <div class="control">
            <input
              class="input"
              type="text"
              placeholder="Surname"
              v-model="lastname"
              required
            />
          </div>
        </div>
      </div>

      <div class="field-body">
        <div class="field">
          <label class="label">Lytis</label>
          <div class="control">
            <label class="radio">
              <input type="radio" name="gender" v-model="gender" value="male" />
              Male
            </label>
            <label class="radio">
              <input
                type="radio"
                name="gender"
                v-model="gender"
                value="female"
              />
              Female
            </label>
            <label class="radio">
              <input
                type="radio"
                name="gender"
                v-model="gender"
                value="other"
              />
              Other
            </label>
          </div>
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
      firstname: "",
      lastname: "",
      gender: "",
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

<style scoped>
.field-body {
  margin-bottom: 1.5em;
}
</style>
