<script setup lang="ts">
import { ref } from 'vue';
import "@mantine/core/styles.css";

// setting the state of which the time is
const state = ref<"stopped" | "running" | "paused">("stopped");

// for the start
const timeElapsed = ref(0);

//for the pause
const interval = ref<number | undefined>(undefined);

function start() {
  state.value = "running";
  interval.value = setInterval(() => {
    timeElapsed.value++;
  }, 1000);
}

function pause() {
  state.value = "paused";
  clearInterval(interval.value);
  interval.value = undefined;
}

function restart () {
  timeElapsed.value = 0;
  if (interval.value !== undefined) {
  clearInterval(interval.value);
  interval.value = undefined;
  }
  start();
}

function formatTime(elaspedTime: number) {
  const minutes = `0${Math.floor(elaspedTime / 60)}`.slice(-2);
  const seconds = `0${elaspedTime % 60}`.slice(-2);
  return `${minutes}:${seconds}`;
}
</script>

<template>
  <div>
    <div class="timeSlot">{{ formatTime(timeElapsed) }}</div>
    <button class="proCol" v-if="state === 'stopped'" @click="start">start</button>
    <button class="stopCol" v-if="state === 'running'" @click="pause">pause</button>
    <button class="proCol" v-if="state === 'paused'" @click="start">resume</button>
    <button class="resCol" v-if="state === 'running' || state === 'paused'" @click="restart">restart</button>
  </div>
</template>

<style scoped>

.timeSlot {
  background: #092327;
  padding: 20px;
  border-radius: 25px;
  margin-bottom: 5px;
  font-size: 50px;
}

.proCol {
  background-color: rgb(13, 75, 13);
  color: #F8F4E3;
}

.stopCol {
  background-color: rgb(111, 17, 17);
  color: #F8F4E3;
}

.resCol {
  background-color: rgb(18, 18, 107);
  color: #F8F4E3;
}

button {
  min-width: 25px;
}

</style>