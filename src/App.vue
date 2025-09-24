<script setup>

import Connect from './components/Connect.vue'
import {ref} from 'vue'
import Cookie from "@/components/Cookie.vue";
import Multiplier from "@/components/Multiplier.vue";
import Clicker from "@/components/Clicker.vue";
import Leaderbord from "@/components/Leaderbord.vue";
const isLogged = ref(false)
const user = ref('')


</script>

<template>
  <div class="bandeau">
    <p1>Learning XP Generator</p1>
  </div>
  <div id="app">
    <Connect v-if="isLogged == false" @success="() => isLogged = true"></Connect>
    <div v-else id="cookie">
      <Cookie></Cookie>
    </div>
  </div>
</template>

<style scoped>
.bandeau {
  background-color: #1d2f50;
  color: white;
  text-align: center;
  font-size: 2em;
  padding: 10px;
  font-family: "Trebuchet MS", sans-serif;
}

#cookie {
  width: 100%;
  padding-top: 20px;
}
</style>


