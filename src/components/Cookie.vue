<script setup>
import {ref, onMounted, computed} from 'vue'
import Clicker from "@/components/Clicker.vue"
import Multiplier from "@/components/Multiplier.vue"
import CircleClicker from "@/components/CircleClicker.vue"
import Stats from "@/components/Stats.vue"
import Leaderbord from "@/components/Leaderbord.vue";
import Presentation from "@/components/Presentation.vue";

const props = defineProps({
  userProgress: {
    type: Object,
    default: () => ({
      userName: 'Nouveau étudiant',
      score: 10000,
      unlockedClickers: [],
      unlockedMultipliers: []
    })
  }
})

const counter = ref(0)
const unlockedClicker = ref([])
const unlockedMultiplier = ref([])
const clickers = ref([])
const multipliers = ref([])

function clickerIncrement(clickers) {
  let totalClicker = 0
  for (let i = 0; i < clickers.value.length; i++) {
    totalClicker += clickers.value[i].clicker * unlockedClicker.value[i]
  }
  return totalClicker
}

function multiplierEffect(multipliers) {
  let totalMultiplier = 1
  for (let i = 0; i < multipliers.value.length; i++) {
    if (unlockedMultiplier.value[i] > 0) {
      totalMultiplier *= multipliers.value[i].multiplier
    }
  }
  return totalMultiplier
}

if (props.userProgress.score > 0) {
  counter.value = props.userProgress.score
}

onMounted(async () => {
  const resClickers = await fetch('/clicker.json')
  clickers.value = await resClickers.json()

  const resMultipliers = await fetch('/multiplier.json')
  multipliers.value = await resMultipliers.json()

  // Initialisation après le fetch
  if (props.userProgress.unlockedClickers.length > 0) {
    unlockedClicker.value = props.userProgress.unlockedClickers
  } else {
    unlockedClicker.value = Array(clickers.value.length).fill(0)
  }

  if (props.userProgress.unlockedMultipliers.length > 0) {
    unlockedMultiplier.value = props.userProgress.unlockedMultipliers
  } else {
    unlockedMultiplier.value = Array(multipliers.value.length).fill(0)
  }
})

function increment() {
  counter.value++
}

const cps = computed(() => clickerIncrement(clickers))
const mEffect = computed(() => multiplierEffect(multipliers))
const generator = computed(() => (cps.value * mEffect.value).toFixed(1))

setInterval(() => {
  counter.value += cps.value * mEffect.value
}, 1000)

function buyClicker(name) {
  const clicker = clickers.value.find(c => c.name === name)
  if (clicker && counter.value >= clicker.cost) {
    const index = clickers.value.indexOf(clicker)
    counter.value -= clicker.cost
    unlockedClicker.value[index] = unlockedClicker.value[index] + 1
    console.log("Achat effectué :", name)
  } else {
    console.log("Achat impossible :", name)
  }
}

function buyMultiplier(name) {
  const multiplier = multipliers.value.find(m => m.name === name)
  if (multiplier) {
    const index = multipliers.value.indexOf(multiplier)
    if (counter.value >= multiplier.cost && unlockedMultiplier.value[index] < 1) {
      counter.value -= multiplier.cost
      unlockedMultiplier.value[index] = 1 // acheté une fois
      console.log("Achat effectué :", name)
    } else {
      console.log("Achat impossible :", name)
    }
  }
}

const spend = computed(() => {
  let total = 0

  for (let i = 0; i < unlockedClicker.value.length; i++) {
    total += clickers.value[i].cost * unlockedClicker.value[i]
  }

  for (let i = 0; i < unlockedMultiplier.value.length; i++) {
    if (unlockedMultiplier.value[i] > 0) {
      total += multipliers.value[i].cost
    }
  }

  console.log(total)
  return total
})
</script>

<template>
  <div id="theGreatContainer">
    <div id="leaderbord">
      <Presentation></Presentation>
      <Leaderbord></Leaderbord>

    </div>

    <div id="gameBoard">
      <div id="greetings">
        <h1>Bienvenue, {{ props.userProgress.userName }} !</h1>
      </div>

      <div id="cookiecontenair">
        <!-- <img class="clickable" src="/caillou.png" alt="cookie.png" @click="increment"/> -->
        <CircleClicker
            :validated="counter"
            :total="20"
            :generator="generator"
            @increment="increment"
        />
      </div>

      <div id="upgrades">
        <div class="colomn">
          <h2>Clickers</h2>
          <Clicker
              v-for="(c, index) in clickers"
              :key="c.name"
              v-bind="c"
              :unlocked="unlockedClicker[index]"
              :userScore="counter.value"
              @buy="buyClicker"
          />
        </div>

        <div class="colomn">
          <h2>Multipliers</h2>
          <Multiplier
              v-for="(m, index) in multipliers"
              :key="m.name"
              v-bind="m"
              :unlocked="unlockedMultiplier[index]"
              :userScore="counter.value"
              @buy="buyMultiplier"
          />
        </div>
      </div>


    </div>

    <div id="stats">
      <Stats :score="counter" :spend="spend" :userName="props.userProgress.userName"
             :unlockedClickers="unlockedClicker" :unlockedMultipliers="unlockedMultiplier" :clickers="clickers"
             :multipliers="multipliers"></Stats>
    </div>


  </div>


</template>


<style scoped>
#theGreatContainer {
  font-family: "Trebuchet MS", sans-serif;
  color: #1d2f50;
  display: flex;
  flex-direction: row;
  align-items: flex-start; /* Aligne les items en haut */
  gap: 20px; /* espace entre les colonnes */
  width: 100%;
}

/* Colonnes avec largeur proportionnelle */
#gameBoard {
  flex: 1;
  max-width: 50%;
  display: flex;
  flex-direction: column;
  align-items: center;
  border: 4px solid #1d2f50;
  background: #f5f7fa;
  padding: 20px;
  border-radius: 12px;
}

#leaderbord {
  flex: 1; /* 50% de l'espace */
  max-width: 25%;
  display: flex;
  flex-direction: column;
  border: 4px solid #1d2f50;
  background: #f5f7fa;
  padding: 20px;
  border-radius: 12px;
}

#stats {
  flex: 1; /* 25% de l'espace */
  max-width: 25%;
  display: flex;
  flex-direction: column;
  border: 4px solid #1d2f50;
  background: #f5f7fa;
  padding: 20px;
  border-radius: 12px;
}

</style>
