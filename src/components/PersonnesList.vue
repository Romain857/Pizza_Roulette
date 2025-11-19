<template>
  <div class="personnes-liste">
    <h3>🙍 Clients du jour</h3>

    <select v-model="selected" :disabled="disabled" class="select-personne">
      <option disabled value="">Sélectionner une personne</option>
      <option v-for="p in personnes" :key="p.nom" :value="p.nom">
        {{ p.nom }}
      </option>
    </select>

    <div v-if="personneCourante" class="details">
      <img
        v-if="personneCourante.photo"
        :src="personneCourante.photo"
        class="avatar"
        alt="Photo de la personne"
      />

      <p v-if="personneCourante.ingredientsDislikes.length">
        ❌ N’aime pas : {{ personneCourante.ingredientsDislikes.join(', ') }}
      </p>
      <p v-else class="ok">👍 Aime tout</p>

      <p class="joker">
        🎟️ {{ personneCourante.joker }} joker<span v-if="personneCourante.joker > 1">s</span>
      </p>
    </div>
  </div>
</template>

<script setup lang="ts">
import { computed } from 'vue'

const props = defineProps<{
  personnes: {
    nom: string
    ingredientsDislikes: string[]
    joker: number
    photo?: string
  }[]
  disabled?: boolean
}>()

const model = defineModel<string | ''>()
const selected = model

const personneCourante = computed(() => props.personnes.find((p) => p.nom === selected.value))
</script>

<style scoped>
.personnes-liste {
  position: absolute;
  top: 1rem;
  right: 2rem;
  background: #fff;
  border: 2px solid #ddd;
  border-radius: 10px;
  padding: 0rem 1.5rem 1rem 1.5rem;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
  width: 260px;
}

.avatar {
  width: 80px;
  height: 80px;
  object-fit: cover;
  border-radius: 50%;
  margin: 0.5rem auto;
  display: block;
  border: 2px solid #ccc;
}

.select-personne {
  width: 100%;
  padding: 10px 12px;
  font-size: 1rem;
  border: 2px solid #ccc;
  border-radius: 8px;
  background: linear-gradient(#fafafa, #f0f0f0);
  cursor: pointer;
  appearance: none;
  margin-bottom: 1rem;
}

h3 {
  text-align: center;
  font-size: 1.2rem;
  margin-bottom: 0.7rem;
}

.details {
  text-align: center;
}

.ok {
  color: #16a34a;
}

.joker {
  font-size: 0.85rem;
  color: #444;
  margin-top: 0.4rem;
}

@media (max-width: 1300px) {
  .personnes-liste {
    position: static;
    width: 90%;
    max-width: 350px;
    margin: 0 auto;
    margin-bottom: 1.5rem;
  }
}
</style>
