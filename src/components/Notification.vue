<template>
  <div class="notification" v-bind:class="type" v-if="display">
    <button class="delete" @click="display = false"></button>
    {{ message }}
  </div>
</template>

<script>
export default {
  name: "Notification",
  props: {
    display: Boolean,
    type: String,
    message: String,
  },
};
</script>
