<template>
  <div class="add-workout">
    <h2 class="subtitle">All workouts</h2>
    <table class="table is-hoverable is-fullwidth">
      <thead>
        <tr>
          <th>Date</th>
          <th>Machine ID</th>
          <th>Weight</th>
          <th>Action</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="workout in workouts" :key="workout.id">
          <td>
            {{ new Date(workout.date.seconds * 1000).toLocaleString("lt") }}
          </td>
          <td>{{ workout.machineId }}</td>
          <td>{{ workout.weight }}</td>
          <td>
            <button class="button is-small" v-on:click="remove(workout.id)">
              Delete
            </button>
          </td>
        </tr>
      </tbody>
    </table>

    <h2 class="subtitle">Add workout</h2>
    <form @submit.prevent="add">
      <div class="field-body">
        <div class="field">
          <label class="label">Machine ID</label>
          <div class="control">
            <input
              class="input"
              type="number"
              placeholder="12"
              v-model="machineId"
              required
            />
          </div>
        </div>
        <div class="field">
          <label class="label">Weight, kg</label>
          <div class="control">
            <input
              class="input"
              type="number"
              placeholder="10"
              v-model="weight"
              required
            />
          </div>
        </div>
      </div>
      <div class="field">
        <div class="control">
          <button class="button is-primary" type="submit">Add Workout</button>
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
  name: "AddWorkout",
  components: {},
  data() {
    return {
      machineId: "",
      weight: "",
      workouts: [],
    };
  },
  methods: {
    add() {
      firebase
        .firestore()
        .collection("users")
        .doc(firebase.auth().currentUser.uid)
        .collection("workouts")
        .add({
          machineId: this.machineId,
          weight: this.weight,
          date: firebase.firestore.FieldValue.serverTimestamp(),
        });
    },
    remove(id) {
      firebase
        .firestore()
        .collection("users")
        .doc(firebase.auth().currentUser.uid)
        .collection("workouts")
        .doc(id)
        .delete();

      this.workouts = this.workouts.filter((workout) => workout.id !== id);
    },
  },
  beforeMount() {
    firebase
      .firestore()
      .collection("users")
      .doc(firebase.auth().currentUser.uid)
      .collection("workouts")
      .orderBy("date", "desc")
      .get()
      .then((snapshot) =>
        snapshot.docs.forEach((doc) =>
          this.workouts.push({
            id: doc.id,
            date: doc.data().date,
            machineId: doc.data().machineId,
            weight: doc.data().weight,
          })
        )
      );
  },
};
</script>

<style scoped>
.field-body {
  margin-bottom: 1.5em;
}
</style>
