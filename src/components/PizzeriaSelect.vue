<template>
  <div class="pizzeria-select">
    <h3>
      <a :href="mapLink" target="_blank" class="maps-link">📍</a>
      Pizzeria
    </h3>

    <select v-model="model" :disabled="props.disabled" class="select-pizzeria">
      <option value="vieux">Au Vieux Pressoir</option>
      <option value="chat">Le Chat qui Guette</option>
    </select>
  </div>
</template>

<script setup lang="ts">
import { computed } from 'vue'

const props = defineProps<{ disabled?: boolean }>()
const model = defineModel<'vieux' | 'chat'>()

const mapsLinks = {
  vieux: 'https://maps.app.goo.gl/nUQXpPXLHHitxFa19',
  chat: 'https://maps.app.goo.gl/XbJrMLw8naPxB7T87',
}

const mapLink = computed(() => mapsLinks[model.value as 'vieux' | 'chat'])
</script>

<style scoped>
.pizzeria-select {
  position: absolute;
  top: 1rem;
  left: 2rem;
  background: #fff;
  border: 2px solid #ddd;
  border-radius: 10px;
  padding: 0rem 1.5rem 1rem 1.5rem;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
  width: 220px;
  text-align: center;
  text-align-last: center;
}

h3 {
  text-align: center;
  font-size: 1.1rem;
  margin-bottom: 0.5rem;
}

.select-pizzeria {
  width: 100%;
  padding: 10px 12px;
  font-size: 1rem;
  border: 2px solid #ccc;
  border-radius: 8px;
  background: linear-gradient(#fafafa, #f0f0f0);
  cursor: pointer;
  appearance: none;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.1);
}

.maps-link {
  display: inline-block;
  margin-top: 0.6rem;
  color: #1a73e8;
  text-decoration: none;
  font-weight: 600;
}

.maps-link:hover {
  text-decoration: underline;
}

@media (max-width: 1300px) {
  .pizzeria-select {
    position: static;
    width: 90%;
    max-width: 350px;
    margin-bottom: 1rem;
  }
}
</style>
