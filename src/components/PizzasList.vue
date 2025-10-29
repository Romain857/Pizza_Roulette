<template>
  <div class="pizza-list">
    <h3>🍕 Pizzas</h3>

    <div class="pizza-section">
      <h4>🍅 Base tomate</h4>
      <ul>
        <li v-for="(pizza, index) in pizzasTomate" :key="'tomate-' + index" class="pizza-item">
          <strong>{{ pizza.nom }}</strong>
          <p class="ingredients">{{ pizza.ingredients }}</p>
        </li>
      </ul>
    </div>

    <div class="pizza-section">
      <h4>🍶 Base crème</h4>
      <ul>
        <li v-for="(pizza, index) in pizzasCreme" :key="'creme-' + index" class="pizza-item">
          <strong>{{ pizza.nom }}</strong>
          <p class="ingredients">{{ pizza.ingredients }}</p>
        </li>
      </ul>
    </div>
  </div>
</template>

<script setup lang="ts">
import { computed } from 'vue'
import { pizzas } from '@/data/Pizzas/pizzas_AuVieuxPressoir'

const pizzasTomate = computed(() => pizzas.filter((p) => p.base === 'tomate'))
const pizzasCreme = computed(() => pizzas.filter((p) => p.base === 'creme'))
</script>

<style scoped>
.pizza-list {
  margin-top: 2rem;
  background: #fff;
  border: 2px solid #ddd;
  border-radius: 10px;
  padding: 1.2rem 1.5rem;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
  width: 90%;
  max-width: 800px;
}

h3 {
  text-align: center;
  font-size: 1.3rem;
  font-weight: bold;
  margin-bottom: 1rem;
}

.pizza-section {
  margin-bottom: 1.5rem;
}

.pizza-section h4 {
  font-size: 1.1rem;
  margin-bottom: 0.5rem;
  border-bottom: 2px solid #eee;
  padding-bottom: 0.3rem;
}

ul {
  list-style: none;
  padding: 0;
  margin: 0;
}

.pizza-item {
  border-bottom: 1px solid #eee;
  padding: 0.5rem 0;
}

.pizza-item:last-child {
  border-bottom: none;
}

strong {
  display: block;
  font-size: 1rem;
  color: #333;
}

.ingredients {
  font-size: 0.9rem;
  color: #555;
  margin-top: 0.2rem;
}

/* Responsive */
@media (max-width: 600px) {
  .pizza-list {
    width: 95%;
    padding: 1rem;
  }

  h3 {
    font-size: 1.1rem;
  }
}
</style>
