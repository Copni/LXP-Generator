<script setup lang="ts">
import {ref} from 'vue'

const props = defineProps({
  userName: String,
  score: Number,
})
const emit = defineEmits(['response'])
// const userName = ref('New User')

</script>

<template>
  <h1>{{userName}}</h1>
</template>

<style scoped>

</style>