<template>
  <input v-model="username" placeholder="Enter username..." @keyup.enter="submitUsername">
</template>

<script setup>

import {ref} from "vue";

const username = ref("");
// eslint-disable-next-line no-undef
const emit = defineEmits(["submittedUsername"]);

function submitUsername() {
  emit("submittedUsername", username.value)
}
</script>

<style scoped>

</style>