<script setup>
import { ref, computed } from "vue"

const props = defineProps({
  validated: { type: Number, default: 0 },  // Points validés
  total: { type: Number, default: 20 } ,
  generator:{type: Number, default: 0} // Points générés par seconde
})

const emit = defineEmits(["increment"])

// rayon et périmètre du cercle
const radius = 90
const circumference = 2 * Math.PI * radius

// progression du cercle (stroke-dashoffset gère l’animation)
const progress = computed(() => {
  const percent = (props.validated % props.total) / props.total
  return circumference * (1 - percent)
})

function handleClick() {
  emit("increment")
}
</script>

<template>
  <div class="circle-container" @click="handleClick">
    <svg class="progress-ring" :width="radius*2+20" :height="radius*2+20">
      <!-- Cercle de fond -->
      <circle
          class="progress-ring__background"
          :r="radius"
          :cx="radius+10"
          :cy="radius+10"
      />
      <!-- Cercle animé -->
      <circle
          class="progress-ring__circle"
          :r="radius"
          :cx="radius+10"
          :cy="radius+10"
          :stroke-dasharray="circumference"
          :stroke-dashoffset="progress"
      />
    </svg>
    <div class="circle-content">
      <p>Points validés</p>
      <h2>{{ validated }}</h2>
      <p>lxp/s</p>
      <b>{{ generator }}</b>
    </div>
  </div>
</template>

<style scoped>
.circle-container {
  position: relative;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
}

.progress-ring__background {
  fill: transparent;
  stroke: #ddd;
  stroke-width: 15;
}

.progress-ring__circle {
  fill: transparent;
  stroke: #3f7f3f;
  stroke-width: 15;
  stroke-linecap: round;
  transform: rotate(-90deg);
  transform-origin: 50% 50%;
  transition: stroke-dashoffset 0.3s ease;
}

.circle-content {
  position: absolute;
  text-align: center;
  font-family: "Trebuchet MS", sans-serif;
}

.circle-content h2 {
  margin: 5px 0;
  font-size: 2rem;
  font-weight: bold;
  color: #1d2f50;
}
</style>
