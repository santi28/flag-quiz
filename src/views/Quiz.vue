<script setup lang="ts">
import { defineAsyncComponent } from 'vue';

import ICountry from '../interfaces/country.interface'
import game from '../game/game.json';

const flags_tag: string[] = game.map(({ flag }) => flag);

const getRandomFlag = (...exluded: string[]) => {
  const filtered_falgs: string[] = flags_tag.filter(flag => !exluded.includes(flag));
  const flag_iso: string = filtered_falgs[Math.floor(Math.random() * filtered_falgs.length)];
  return game.find(({ flag }) => flag === flag_iso);
};


const answer_flag_1: ICountry | undefined = getRandomFlag();
const answer_flag_2: ICountry | undefined = getRandomFlag(answer_flag_1?.flag||'');
const answer_flag_3: ICountry | undefined = getRandomFlag(answer_flag_1?.flag||'', answer_flag_2?.flag||'');


if (!answer_flag_1 || !answer_flag_2 || !answer_flag_3) 
  throw new Error('Ups! an error occured when generating the flag');

// Randomize in array getted country names
const country_names: {name: string, correct: boolean}[] = [
  {name: answer_flag_1.country, correct: true},
  {name: answer_flag_2.country, correct: false},
  {name: answer_flag_3.country, correct: false},
];
country_names.sort(() => Math.random() - 0.5);

/// <reference path="../index.d.ts" />
const Flag = defineAsyncComponent(() => import(`../game/flags/${answer_flag_1.flag}.svg`));

const responseHandler = (response: string) => {
  const is_correct = country_names.find(({ name }) => name === response)?.correct;
  if (is_correct) {
    alert('Correct!');
  } else {
    alert('Wrong!');
  }
};
</script>

<template>
  <div class="flag_wrapper">
    <Flag class="flag"/>
  </div>
  <div class="button_wrapper">
    <button v-for="{ name }, index of country_names" :key="index" @click="responseHandler(name)">{{name}}</button>
  </div>
</template>

<style scoped>
.flag_wrapper {
  display: flex;
  justify-content: center;
  align-items: center;
}

.flag {
  width: 195%;
  max-width: 700px;
  background-color: #221E26;
}

.button_wrapper {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 25px;
}

button {
  background-color: whitesmoke;
  border: none;

  font-size: 1.8rem;
  font-family: 'Outfit', sans-serif;
  font-weight: bold;
  text-align: center;

  padding: 25px 60px;
  border-radius: 10px;
  cursor: pointer;
}
</style>
