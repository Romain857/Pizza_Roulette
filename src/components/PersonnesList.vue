<template>
  <div class="personnes-liste">
    <h3>👥 Joueurs</h3>
    <ul>
      <li v-for="(personne, index) in personnes" :key="index" class="personne-item">
        <label class="personne-label">
          <input type="checkbox" v-model="personne.present" />
          <strong>{{ personne.nom }}</strong>
        </label>

        <p v-if="personne.ingredientsDislikes.length">
          ❌ {{ personne.ingredientsDislikes.join(', ') }}
        </p>
        <p v-else class="ok">👍 Aime tout</p>
      </li>
    </ul>
  </div>
</template>

<script setup lang="ts">
import { personnes } from '@/data/personnes'
</script>

<style scoped>
.personnes-liste {
  position: absolute;
  top: 1rem;
  right: 2rem;
  background: #fff;
  border: 2px solid #ddd;
  border-radius: 10px;
  padding: 1rem 1.5rem;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
  width: 220px;
}

h3 {
  text-align: center;
  margin-bottom: 0.5rem;
  font-size: 1.1rem;
}

ul {
  list-style: none;
  margin: 0;
  padding: 0;
}

.personne-item {
  border-bottom: 1px solid #eee;
  padding: 0.5rem 0;
}

.personne-item:last-child {
  border-bottom: none;
}

.personne-label {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  cursor: pointer;
}

.personne-label input[type='checkbox'] {
  width: 18px;
  height: 18px;
  accent-color: #3b82f6;
  cursor: pointer;
}

strong {
  font-size: 1rem;
  color: #333;
}

p {
  font-size: 0.9rem;
  color: #555;
  margin: 0.2rem 0 0;
}

.ok {
  color: #16a34a;
}

@media (max-width: 1100px) {
  .personnes-liste {
    position: static;
    width: 90%;
    max-width: 350px;
    margin-top: 1rem;
  }
}
</style>