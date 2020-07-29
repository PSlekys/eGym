<template>
  <div class="settings">
    <h2 class="subtitle">Settings</h2>
    <form>
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
export default {
  name: "Settings",
  data() {
    return {
      name: "",
    };
  },
};
</script>
