<script setup lang="ts">
import { ref } from 'vue';
import Stopwatch from './components/Stopwatch.vue';
import Countdown from './components/Countdown.vue';

// Track the selected widget
const selectedWidget = ref('stopwatch');

// Function to change the selected widget
function selectWidget(widget: string) {
  selectedWidget.value = widget;
}
</script>

<template>
  <div>
    <!-- Buttons to select widget -->
    <div class="widget-selector">
      <button class="wButton" @click="selectWidget('stopwatch')" :style="{ backgroundColor: selectedWidget === 'stopwatch' ? 'green' : 'red' }">
        Stopwatch
      </button>
      <button class="wButton" @click="selectWidget('countdown')" :style="{ backgroundColor: selectedWidget === 'countdown' ? 'green' : 'red' }">
        Countdown
      </button>
    </div>

    <!-- Conditionally render selected component -->
    <div v-if="selectedWidget === 'stopwatch'" class="widget-container">
      <Stopwatch />
    </div>
    <div v-if="selectedWidget === 'countdown'" class="widget-container">
      <Countdown />
    </div>
  </div>
</template>

<style scoped>

.wButton {
  margin: 5px;
}

/* Fixed position for buttons at the top */
.widget-selector {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  display: flex;
  justify-content: center;
  padding: 10px 0;
  border-bottom: 2px solid #ddd;
  z-index: 1000;
}

/* Adjust widget container margins to compensate for the fixed buttons */
.widget-container {
  margin-top: 60px;
  padding: 20px;
}
</style>

