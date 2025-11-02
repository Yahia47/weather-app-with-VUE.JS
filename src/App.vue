<template>
  <main>
    <!-- Date -->
    <div class="text-center mb-6">
      {{
        new Date().toLocaleDateString('en-us', {
          weekday: 'long',
          year: 'numeric',
          month: 'long',
          day: 'numeric',
          hour: 'numeric',
          minute: 'numeric',
          second: 'numeric',
        })
      }}
    </div>
    <!-- Search -->
    <div>
      <SearchInput @place-data="addPlace" />
    </div>
    <!-- Weather cards -->
    <div class="grid grid-cols-2 gap-4">
      <div v-for="(place, idx) in places" :key="idx">
        <WeatherCard :place="place" @delete-place="deletePlace" />
      </div>
    </div>
  </main>
</template>

<script setup>
import { ref } from 'vue'
import SearchInput from './components/Search-input.vue'
import WeatherCard from './components/weather-card.vue'
const places = ref([])

const addPlace = (data) => {
  places.value.push(data)
}
const deletePlace = (name) => {
  if (confirm('Are you sure to delete this place ?')) {
    places.value = places.value.filter((p) => p.location.name !== name)
  }
}
</script>

