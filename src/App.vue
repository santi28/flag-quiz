<script setup lang="ts">
// Nodemodules imports
import { defineAsyncComponent, onErrorCaptured, onMounted, ref } from 'vue';

// Workdir imports
/* eslint-disable-next-line */
import Quiz from './views/Quiz.vue';


let error = ref<Error|null>(null);
let response = ref<{ success: boolean, message: string }|null>(null);
let score = ref<number>(0);

onErrorCaptured(e => {
  error.value = e;
})

const answerHandler = (isCorrect: boolean, message: string) => {
  if (isCorrect) { 
    score.value += 100;
    response.value = { success: true, message: 'Ohh, Yeha!' }
  } else {
    response.value = { success: false, message: 'Nope!' }
  };

  setTimeout(() => { response.value = null; }, 1000);
}
</script>

<template>
  <h2>{{score}}</h2>
  <div v-if="error">{{ error }}</div>
  <Suspense>
    <template #default>
      <Quiz v-if="!response" @answer="answerHandler"/>
    </template>
    <template #fallback>
      <div class="answer_mesage">Loading...</div>
    </template>
  </Suspense>
  <div 
    v-if="response"
    class="answer_mesage"
    :class="{ correct: response.success, incorrect: !response.success }">
    {{ response.message }}
  </div>
</template>

<style>
* {
  margin: 0;
  padding: 0;
}

body, html {
  height: 100%;
  width: 100%;
  background-color: #221E26;
}

h2 {
  color: #fff;
  font-size: 2rem;
  text-align: center;
  padding-top: 50px;
  font-family: 'Outfit', sans-serif;
}

.answer_mesage {
  font-family: 'Outfit', sans-serif;
  font-size: 3rem;
  font-weight: bold;
  text-align: center;
  padding-top: 35px;
  color: #fff;
}

.answer_mesage.correct {
  color: #21F95D;
}

.answer_mesage.incorrect {
  color: #FF4F4F;
}
</style>
