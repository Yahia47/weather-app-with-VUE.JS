<script setup>
import { reactive } from 'vue'
const emit = defineEmits(['place-data'])

const searchTerm = reactive({
  query: '',
  timeout: null,
  results: [],
})

const handleSearch = () => {
  clearTimeout(searchTerm.timeout)

  searchTerm.timeout = setTimeout(async () => {
    if (searchTerm.query !== '') {
      const res = await fetch(
        `http://api.weatherapi.com/v1/search.json?key=f96ebafa18184740997125824250111&q=${searchTerm.query}`
      )
      searchTerm.results = await res.json()
    } else {
      searchTerm.results = []
    }
  }, 500)
}

const getWeather = async (id) => {
  const res = await fetch(
    `http://api.weatherapi.com/v1/forecast.json?key=f96ebafa18184740997125824250111&q=id:${id}&days=3&aqi=no&alerts=no`
  )
  const data = await res.json()
  emit('place-data', data)
  // console.log(data)

  searchTerm.query = ''
  // searchTerm.results = null
  searchTerm.results = []
}
</script>

<template>
  <div>
    <!-- search field -->
    <form>
      <div class="bg-white border border-indigo-600/30 rounded-lg shadow-lg flex items-center">
        <i class="fa-solid fa-magnifying-glass p-2 text-indigo-600"></i>
        <input
          type="text"
          placeholder="Search for a place"
          class="rounded-r-lg p-2 border-0 outline-0 focus:ring-2 focus:ring-indigo-600 ring-inset w-full"
          v-model="searchTerm.query"
          @input="handleSearch"
        />
      </div>
    </form>

    <!-- search suggestions -->
    <div v-if="searchTerm.results.length > 0" class="bg-white my-2 rounded-lg shadow-lg">
      <div v-for="city in searchTerm.results" :key="city.id">
        <button
          @click="getWeather(city.id)"
          class="px-3 my-2 hover:text-indigo-600 hover:font-bold w-full text-left"
        >
          {{ city.name }} - {{ city.region }} , {{ city.country }}
        </button>
      </div>
    </div>
  </div>
</template>
