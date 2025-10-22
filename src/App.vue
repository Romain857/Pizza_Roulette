<template>
  <div class="roulette-container">
    <h2>🚢 Quelle pizza du Vieux Pressoir?</h2>

    <div class="roulette-wrapper">
      <div class="arrow"></div>
      <div class="roulette-window">
        <div class="roulette-list" :style="{ transform: `translateY(-${position}px)` }">
          <div v-for="(pizza, index) in pizzas" :key="'1-' + index" class="roulette-item">
            {{ pizza.nom }}
          </div>
          <div v-for="(pizza, index) in pizzas" :key="'2-' + index" class="roulette-item">
            {{ pizza.nom }}
          </div>
        </div>
      </div>
    </div>

    <button @click="lancerRoulette" :disabled="enCours">🎲 Lance !</button>

    <div v-if="resultat" class="resultat">
      🍕 <strong>{{ resultat }}</strong>
      <p class="ingredients">{{ ingredients }}</p>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import { pizzas } from '@/data/pizzas'

const position = ref(0)
const resultat = ref('')
const ingredients = ref('')
const enCours = ref(false)

const itemHeight = 60
const windowHeight = 300

const lancerRoulette = () => {
  if (enCours.value) return
  enCours.value = true
  resultat.value = ''
  ingredients.value = ''

  const total = pizzas.length
  const tours = 20
  const randomIndex = Math.floor(Math.random() * total)
  const centerOffset = (windowHeight - itemHeight) / 2
  const distance = (tours * total + randomIndex) * itemHeight - centerOffset

  const duration = 4000
  const start = performance.now()

  const animate = (time: number) => {
    const progress = Math.min((time - start) / duration, 1)
    const ease = 1 - Math.pow(1 - progress, 3)
    position.value = (distance * ease) % (total * itemHeight)

    if (progress < 1) {
      requestAnimationFrame(animate)
    } else {
      const pizzaChoisie = pizzas[randomIndex]
      resultat.value = pizzaChoisie?.nom ?? ''
      ingredients.value = pizzaChoisie?.ingredients ?? ''
      enCours.value = false
    }
  }

  requestAnimationFrame(animate)
}
</script>

<style scoped>
.roulette-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  font-family: sans-serif;
  text-align: center;
  padding: 2rem 0;
  gap: 1.5rem;
}

/* TITRE */
h2 {
  font-size: 1.8rem;
  font-weight: bold;
  margin-bottom: 0.5rem;
}

.roulette-wrapper {
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
}

.arrow {
  position: absolute;
  top: 50%;
  left: calc(50% + 130px);
  transform: translate(-50%, -50%) rotate(90deg);
  width: 0;
  height: 0;
  border-left: 12px solid transparent;
  border-right: 12px solid transparent;
  border-top: 20px solid red;
  z-index: 2;
}

.roulette-window {
  width: 260px;
  height: 300px;
  border: 3px solid #333;
  border-radius: 10px;
  overflow: hidden;
  background: #ffffff;
  box-shadow: 0 6px 16px rgba(0, 0, 0, 0.25);
}

.roulette-list {
  transition: transform 0.2s linear;
}

.roulette-item {
  height: 60px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: 500;
  font-size: 1.1rem;
  border-bottom: 1px solid #eee;
  background: #fafafa;
}

button {
  background: #ff4d4d;
  color: white;
  border: none;
  padding: 12px 25px;
  font-size: 1.1rem;
  cursor: pointer;
  border-radius: 8px;
  transition:
    background 0.25s ease,
    transform 0.1s ease;
}

button:hover:not(:disabled) {
  background: #e63b3b;
  transform: scale(1.05);
}

button:disabled {
  opacity: 0.6;
  cursor: not-allowed;
}

.resultat {
  font-size: 1.3rem;
  margin-top: 0.5rem;
  color: #333;
}

.ingredients {
  margin-top: 0.3rem;
  font-size: 1rem;
  color: #555;
  max-width: 400px;
}
</style>
