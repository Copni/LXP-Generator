<script setup>
const props = defineProps({
  name : String,
  cost : Number,
  multiplier : Number,
  description : String,
  unlocked : Boolean,
  picture : String
})

const emit = defineEmits(["buy"]);

function sendBuy(){
  emit("buy", props.name);
}
</script>

<template>
  <div class="multiplier">
    <img :src="picture" alt="Multiplier Image" />
    <div class="description">
      <h3>{{ name }} (x{{ multiplier }})</h3>
      <p>{{ description }}</p>
      <p><strong>Coût </strong>: {{ cost }} points LXP</p>
      <p><strong>Débloqué</strong> : {{unlocked}}</p>
      <button :disabled="cost < userScore" @click="sendBuy">Acheter</button>
    </div>

  </div>
</template>
<style scoped>
.multiplier {
  display: flex;
  align-items: center;
  background: #ffffff; /* blanc */
  border: 2px solid #e0e6ed; /* gris doux */
  border-radius: 12px;
  padding: 12px;
  margin: 12px 0;
  box-shadow: 0 2px 6px rgba(0,0,0,0.1);
  color: #1d2f50; /* texte bleu foncé */
  font-family: "Trebuchet MS", sans-serif;
}

.multiplier img {
  width: 64px;
  height: 64px;
  margin-right: 12px;
  border-radius: 8px;
  border: 2px solid #e0e6ed;
  background: #f5f7fa;
}

.description h3 {
  margin: 0;
  font-size: 1.1rem;
  font-weight: bold;
  color: #1d2f50; /* bleu foncé */
}

.description p {
  margin: 4px 0;
  font-size: 0.9rem;
  color: #333;
}

button {
  margin-top: 6px;
  padding: 6px 12px;
  border: none;
  border-radius: 8px;
  background: #2f80ed; /* bleu clair */
  color: #fff;
  font-weight: bold;
  cursor: pointer;
  box-shadow: 0 2px 4px rgba(0,0,0,0.2);
  transition: transform 0.1s, background 0.2s;
}

button:hover:enabled {
  background: #256bcc; /* bleu un peu plus foncé */
  transform: scale(1.05);
}

button:disabled {
  background: #a0aec0; /* gris */
  cursor: not-allowed;
}
</style>