<script setup lang="ts">
// Nodemodules imports
import { defineAsyncComponent, onErrorCaptured, onMounted, ref } from 'vue';

// Workdir imports
/* eslint-disable-next-line */
import Quiz from './views/Quiz.vue';


let error = ref<Error | null>(null);

onErrorCaptured(e => {
  error.value = e;
})
</script>

<template>
  <div v-if="error">{{ error }}</div>
  <Quiz />
</template>

<style>
* {
  margin: 0;
  padding: 0;
}

body, html {
  height: 100%;
  width: 100%;
}

#app {
  width: 100%;
  height: 100%;
  background-color: #221E26;
}
</style>
