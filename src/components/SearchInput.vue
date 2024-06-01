<script setup>
import { reactive } from 'vue'
const emits =defineEmits(['place-data'])
const searchterm = reactive({
  query: '',
  timeout: null,
  result: ''
})

const handleSearch = () => {
  clearTimeout(searchterm.timeout)
  searchterm.timeout = setTimeout(async () => {
    if (searchterm.query !== '') {
      const response = await fetch(
        `http://api.weatherapi.com/v1/search.json?key=32941cc213424101846125810240106&q=${searchterm.query}`
      )
      const data = await response.json()
      searchterm.result = data
    } else {
      searchterm.result = null
    }
  }, 500)
}

const getWeather = async (id) => {
  const res = await fetch(
    `http://api.weatherapi.com/v1/forecast.json?key=32941cc213424101846125810240106&q=id:${id}&days=3&aqi=no&alerts=no`
  )
  const data =await res.json()
  emits('place-data', data)
  searchterm.query=''
  searchterm.result=null

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
          v-model="searchterm.query"
          @input="handleSearch"
        />
      </div>
    </form>
    <!-- search suggestions -->
    <div class="bg-white my-2 rounded-lg shadow-lg">
      <div v-if="searchterm.result !== null">
        <div v-for="place in searchterm.result" :key="place.id">
          <button
            @click="getWeather(place.id)"
            class="px-3 my-2 hover:text-indigo-600 hover:font-bold w-full text-left"
          >
            {{ place.name }},{{ place.region }},{{ place.country }}
          </button>
        </div>
      </div>
    </div>
  </div>
</template>
