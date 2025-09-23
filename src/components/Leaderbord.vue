<template>
  <div class="leaderboard">
    <h2>🏆 Classement</h2>
    <ul>
      <li
          v-for="(user, index) in topUsers"
          :key="user.username"
          class="leaderboard-item"
      >
        <span class="rank">#{{ index + 1 }}</span>
        <span class="username">{{ user.username }}</span>
        <span class="score">{{ user.score }}</span>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue"

const users = ref([])
const topUsers = ref([])

onMounted(async () => {
  try {
    const response = await fetch("/users.json")
    const data = await response.json()
    users.value = data

    // Trier par score décroissant et garder les 10 premiers
    topUsers.value = [...users.value]
        .sort((a, b) => b.score - a.score)
        .slice(0, 10)
  } catch (error) {
    console.error("Erreur de chargement users.json :", error)
  }
})
</script>

<style scoped>
.leaderboard {
  background: #ffffff;
  border: 2px solid #e0e6ed;
  border-radius: 12px;
  padding: 20px;
  max-width: 400px;
  margin: 20px auto;
  box-shadow: 0 2px 6px rgba(0,0,0,0.1);
  font-family: "Trebuchet MS", sans-serif;
}

.leaderboard h2 {
  text-align: center;
  color: #1d2f50; /* bleu foncé */
  margin-bottom: 16px;
}

ul {
  list-style: none;
  padding: 0;
  margin: 0;
}

.leaderboard-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 8px 12px;
  border-bottom: 1px solid #e0e6ed;
  font-size: 1rem;
  color: #1d2f50;
}

.leaderboard-item:last-child {
  border-bottom: none;
}

.rank {
  font-weight: bold;
  color: #2f80ed; /* bleu clair accent */
  margin-right: 8px;
}

.username {
  flex: 1;
  text-align: left;
}

.score {
  font-weight: bold;
  color: #1d2f50;
}
</style>
