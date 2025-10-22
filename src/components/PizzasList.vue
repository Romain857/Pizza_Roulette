<template>
  <div class="pizza-list">
    <h3>🍕 Pizzas</h3>
    <ul>
      <li v-for="(pizza, index) in pizzas" :key="index" class="pizza-item">
        <strong>{{ pizza.nom }}</strong>
        <p>{{ pizza.ingredients }}</p>
      </li>
    </ul>
  </div>
</template>

<script setup lang="ts">
import { pizzas } from '@/data/pizzas'
</script>

<style scoped>
.pizza-list {
  top: 1rem;
  left: 2rem;
  background: #fff;
  border: 2px solid #ddd;
  border-radius: 10px;
  padding: 1rem 1.5rem;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
}

.pizza-list h3 {
  text-align: center;
  margin-bottom: 0.5rem;
  font-size: 1.1rem;
}

.pizza-item {
  border-bottom: 1px solid #eee;
  padding: 0.5rem 0;
}

.pizza-item:last-child {
  border-bottom: none;
}

.pizza-item strong {
  display: block;
  font-size: 1rem;
  color: #333;
}

.pizza-item p {
  font-size: 0.9rem;
  color: #555;
  margin-top: 0.2rem;
}

@media (max-width: 1100px) {
  .pizza-list {
    position: static;
    width: 90%;
    max-width: 350px;
  }
}
</style>