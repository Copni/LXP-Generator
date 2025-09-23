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
  <div>
    <Connect v-if="isLogged == false" @success="() => isLogged = true"></Connect>
    <div v-else class="container">
      <div class="cookie">
        <Cookie></Cookie>
      </div>
      <div class="leaderboard">
        <Leaderbord></Leaderbord>
      </div>
    </div>
  </div>
</template>

<style scoped>
.container {
  display: flex;
  flex-direction: row;
  justify-content: space-evenly;  /* espace entre Cookie et Leaderboard */
  align-items: flex-start;
  gap: 40px;
  margin-top: 30px;
  width: 100%;                     /* prend toute la largeur */
  max-width: 100vw;                /* ne dépasse jamais l’écran */
  box-sizing: border-box;          /* évite les débordements */
  padding: 0 20px;                 /* petit padding sur les côtés */
}


.bandeau {
  background-color: #1d2f50;
  color: white;
  text-align: center;
  font-size: 2em;
  padding: 10px;
  font-family: "Trebuchet MS", sans-serif;
}
</style>


