<script setup lang="ts">
import { ref, watch, computed, Ref } from 'vue';

const hours = ref(0);
const minutes = ref(0);
const seconds = ref(0);

// Combine hours, minutes, and seconds to calculate the starting time in seconds
const startingTime: Ref<number> = computed(() => hours.value * 3600 + minutes.value * 60 + seconds.value);
const currentTime = ref(startingTime.value); // Create a separate ref for current time

// setting the state of which the time is
const state = ref<"stopped" | "running" | "paused">("stopped");

const interval = ref<number | undefined>(undefined);

  function start() {
  state.value = "running";
  currentTime.value = startingTime.value; // Initialize currentTime with startingTime
  interval.value = setInterval(() => {
    currentTime.value = Math.max(0, currentTime.value - 1); // time doesn't go negative
  }, 1000);
}

function pause() {
  state.value = "paused";
  // pause the interval
  clearInterval(interval.value);
}

function resume() {
  state.value = "running";
  interval.value = setInterval(() => {
    currentTime.value = Math.max(0, currentTime.value - 1); // time doesn't go negative
  }, 1000);
}

function reset() {
  state.value = "stopped";
  clearInterval(interval.value);
  hours.value = 0;
  minutes.value = 0;
  seconds.value = 0;
  currentTime.value = 0;
}

// Watch for changes in the startingTime, and reset the timer if it reaches 0
watch(currentTime, (newVal) => {
  if (newVal <= 0) {
    reset();
  }
});

function formatTime(time: number) {
  const hours = `0${Math.floor(time / 3600)}`.slice(-2);
  const minutes = `0${Math.floor((time % 3600) / 60)}`.slice(-2);
  const seconds = `0${time % 60}`.slice(-2);
  return `${hours}:${minutes}:${seconds}`;
}

</script>

<template>
  <div>
    <h1 class="mainHead">Final Countdown</h1>
    <h2 class="timerBox" v-if="state === 'running' || state === 'paused'">{{ formatTime(currentTime) }}</h2>
    <div>
      <label class="labels" for="hours">Hour(s):</label>
      <input class="inputs" type="number" id="hours" v-model="hours" min="0" max="24" />
      <label class="labels" for="minutes">Minute(s):</label>
      <input class="inputs" type="number" id="minutes" v-model="minutes" min="0" max="60" />
      <label class="labels" for="seconds">Second(s):</label>
      <input class="inputs" type="number" id="seconds" v-model="seconds" min="0" max="60" />
    </div>
    <div class="but-div">
      <button class="startClick" v-if="state === 'stopped'" @click="start">Start</button>
      <button class="pauseClick" v-if="state === 'running'" @click="pause">Pause</button>
      <button class="restartClick" v-if="state === 'paused'" @click="start">Restart Timer</button>
      <button class="startClick" v-if="state === 'paused'" @click="resume">Resume</button>
      <button class="resetClick" v-if="state === 'running' || state === 'paused'" @click="reset">Reset</button>
    </div>
  </div>
</template>


<style scoped>

.but-div {
  margin-top: 15px;
}

.mainHead {
  background-color: #0A210F;
  padding: 15px;
  border-radius: 10px;
  color: #B0B57D;
}

.timerBox {
  color: #0A210F;
  font-size: 40px;
}

.labels {
  background-color: #0A210F;
  padding: 5px;
  color: #B0B57D;
  border-radius: 10px;
}

.inputs {
  padding: 5px;
  border-radius: 10px;
  width: 50px;
  background-color: #B0B57D;
  color: #0A210F;
}

.startClick {
  background-color: #0A210F;
  color: #B0B57D;
}

.pauseClick {
  background-color: rgb(84, 1, 1);
  color: #B0B57D;
}

.restartClick {
  background-color: #B0B57D;
  color: #0A210F;
}

.resetClick {
  background-color: rgb(0, 0, 71);
  color: #B0B57D;
}

</style>
