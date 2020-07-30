<template>
  <div class="notification" v-bind:class="type" v-if="display">
    <button class="delete" @click="close"></button>
    {{ message }}
  </div>
</template>

<script>
export default {
  name: "Notification",
  props: {
    type: String,
    message: String,
  },
  methods: {
    close() {
      this.$emit("close");
    },
  },
};
</script>
