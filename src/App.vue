<template>
  <div class="app-container">
    <PizzeriaSelect v-model="selectedPizzeria" :disabled="enCours" />
    <PersonnesList v-model="selectedName" :personnes="personnes" :disabled="enCours" />

    <div class="roulette-container">
      <h2>🚢 Pizza Roulette – {{ nomPizzeria }} 🍕</h2>

      <div class="roulette-wrapper">
        <div class="arrow"></div>
        <div class="roulette-window">
          <div class="roulette-list" :style="{ transform: `translateY(-${position}px)` }">
            <div
              v-for="(pizza, index) in [...filteredPizzas, ...filteredPizzas]"
              :key="index"
              class="roulette-item"
            >
              <span class="pizza-name">
                {{ pizza.nom }}
                <span
                  class="base-icon"
                  :title="pizza.base === 'tomate' ? 'Base tomate' : 'Base crème'"
                >
                  {{ pizza.base === 'tomate' ? '🍅' : '🍶' }}
                </span>
              </span>
            </div>
          </div>
        </div>
      </div>

      <button @click="lancerRoulette" :disabled="enCours">Lance !</button>

      <div v-if="resultat" class="resultat">
        <div class="resultat-header">
          <img
            v-if="personneSelectionnee?.photo"
            :src="personneSelectionnee.photo"
            alt="Photo"
            class="avatar-resultat"
          />
          <span>
            {{ personneSelectionnee?.photo ? '' : 'Tu' }}
            a tiré la :
            <strong>{{ resultat }}</strong> !
          </span>
        </div>

        <p class="ingredients">{{ ingredients }}</p>

        <div v-if="personneSelectionnee" class="joker-info">
          <template v-if="personneSelectionnee.joker > 0">
            🎟️ Il te reste
            <strong>{{ personneSelectionnee.joker }}</strong> joker<span
              v-if="personneSelectionnee.joker > 1"
              >s</span
            >.
            <br />
            Tu peux <strong>relancer</strong> ou <strong>prendre la pizza du jour</strong> 😄
          </template>

          <template v-else>
            😈 Plus de joker…
            <br />
            <strong>Paye la tournée</strong> de boisson avoir et utiliser un joker !
          </template>
        </div>
      </div>
      <PizzasList class="pizzas-liste" :pizzas="pizzasActives" />
    </div>
  </div>
</template>

<script setup lang="ts">
import { computed, ref, watch } from 'vue'
import PizzeriaSelect from '@/components/PizzeriaSelect.vue'
import { pizzas as pizzasVieux } from '@/data/Pizzas/pizzas_AuVieuxPressoir'
import { pizzas as pizzasChat } from '@/data/Pizzas/pizzas_LeChatQuiGuette'
import PersonnesList from '@/components/PersonnesList.vue'
import PizzasList from '@/components/PizzasList.vue'
import { personnes as personnesData } from '@/data/personnes'

const personnes = ref(personnesData)
const selectedName = ref('')
const position = ref(0)
const resultat = ref('')
const ingredients = ref('')
const enCours = ref(false)

const itemHeight = 60
const windowHeight = 300

const selectedPizzeria = ref<'vieux' | 'chat'>('vieux')

const pizzasActives = computed(() =>
  selectedPizzeria.value === 'vieux' ? pizzasVieux : pizzasChat,
)

const nomPizzeria = computed(() => {
  return selectedPizzeria.value === 'vieux' ? 'Aux Vieux Pressoir' : 'Le Chat Qui Guette'
})

const personneSelectionnee = computed(() =>
  personnes.value.find((p) => p.nom === selectedName.value),
)

const filteredPizzas = computed(() => {
  const p = personneSelectionnee.value

  const baseList = p
    ? pizzasActives.value.filter(
        (pizza) =>
          !p.ingredientsDislikes.some((bad) =>
            pizza.ingredients.toLowerCase().includes(bad.toLowerCase()),
          ),
      )
    : pizzasActives.value

  return [...baseList].sort(() => Math.random() - 0.5)
})

const lancerRoulette = () => {
  if (enCours.value) return
  enCours.value = true
  resultat.value = ''
  ingredients.value = ''

  const total = filteredPizzas.value.length
  const randomIndex = Math.floor(Math.random() * total)
  const tours = 20
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
      const pizzaChoisie = filteredPizzas.value[randomIndex]
      resultat.value = pizzaChoisie?.nom ?? ''
      ingredients.value = pizzaChoisie?.ingredients ?? ''
      enCours.value = false
    }
  }

  requestAnimationFrame(animate)
}

watch(selectedName, () => {
  resultat.value = ''
  ingredients.value = ''
})
</script>

<style scoped>
.app-container {
  position: relative;
  display: flex;
  justify-content: center;
  align-items: flex-start;
  padding-top: 2rem;
  font-family: sans-serif;
}

.roulette-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
  gap: 1.5rem;
}

h2 {
  font-size: 1.8rem;
  font-weight: bold;
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

.joker-info {
  margin-top: 0.5rem;
  background: #fff7d1;
  border-left: 4px solid #fbbf24;
  padding: 0.7rem 1rem;
  border-radius: 6px;
  font-size: 0.95rem;
  color: #7a5c00;
}

.resultat-header {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.7rem;
  margin-bottom: 0.5rem;
}

.avatar-resultat {
  width: 55px;
  height: 55px;
  border-radius: 50%;
  object-fit: cover;
  border: 2px solid #ddd;
}

@media (max-width: 1300px) {
  .app-container {
    flex-direction: column;
    align-items: center;
    padding: 1.5rem;
    gap: 2rem;
  }

  h2 {
    font-size: 1.4rem;
  }
}
</style>
