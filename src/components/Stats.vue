<script setup>

const props = defineProps({
  userName: String,
  score: Number,
  spend: Number,
  unlockedClickers: Array,
  unlockedMultipliers: Array,
  clickers: Array,
  multipliers: Array
})

</script>

<template>
  <div class="stats-container">
    <h1>Statistiques de l'élève {{ props.userName }}</h1>
    <p>LXP possédés: {{ props.score }}</p>
    <p>LXP dépensés : {{ props.spend }}</p>
    <p>Total généré : {{ props.spend + props.score}}</p>
    <p>Semestre validé :  {{Math.round((props.spend + props.score)/20)}}</p>

    <h2>Clickers possédés :</h2>
    <ul>
      <li v-for="(count, index) in props.unlockedClickers" :key="index">
        {{ props.clickers[index]?.name || 'Clicker ' + (index + 1) }}
        <span>{{ count }}</span>
      </li>
    </ul>

    <h2>Multiplicateurs possédés :</h2>
    <ul>
      <li v-for="(count, index) in props.unlockedMultipliers" :key="index">
        {{ props.multipliers[index]?.name || 'Multiplicateur ' + (index + 1) }}
        <span>{{ count }}</span>
      </li>
    </ul>
  </div>
</template>


<style scoped>
.stats-container {
  background: #fff;
  border-radius: 12px;
  padding: 20px;
  margin: 20px auto;
  max-width: 700px;
  box-shadow: 0 2px 6px rgba(0,0,0,0.1);
}

.stats-container h1 {
  font-size: 1.6rem;
  text-align: center;
  margin-bottom: 20px;
  color: #2c3e50;
}

.stats-container h2 {
  font-size: 1.2rem;
  margin-top: 20px;
  margin-bottom: 10px;
  color: #34495e;
  border-bottom: 1px solid #eee;
  padding-bottom: 5px;
}

.stats-container p {
  font-size: 1rem;
  margin: 5px 0;
  color: #555;
}

.stats-container ul {
  list-style: none;
  padding: 0;
  margin: 0;
}

.stats-container li {
  background: #f9f9f9;
  margin-bottom: 8px;
  padding: 10px 15px;
  border-radius: 8px;
  box-shadow: 0 1px 3px rgba(0,0,0,0.05);
  display: flex;
  justify-content: space-between;
  align-items: center;
  font-size: 0.95rem;
  color: #333;
}

.stats-container li:hover {
  background: #eef5ff;
}

</style>