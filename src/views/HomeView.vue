<template>
  <main class="container text-white">
    <div class="pt-4 mb-8 relative">
      <input type="text"
             v-model="searchQuery"
             @input="showSearchResult"
             placeholder="Search for a city or state"
             class="w-full bg-transparent px-2 py-1 border-b focus:border-weather-secondary focus:outline-none focus:shadow-[0px_1px_0_0_#004E71]">

      <ul v-if="searchResults" class="absolute bg-weather-secondary text-white w-full shadow-md py-2 px-1 top-[45px]">
        <p class="px-2 py-1" v-if="searchError"> Sorry, something went wrong, please try again.</p>
        <p class="px-2 py-1" v-if="!searchError && searchResults.length === 0"> No results match your query, try a
          different term.</p>
        <template v-else>
          <li class="px-2 py-1 cursor-pointer" v-for="result in searchResults" :key="result.id"
              @click="previewCity(result)">
            {{ result.place_name }}
          </li>
        </template>
      </ul>

    </div>
  </main>
</template>

<script setup>
import {ref} from "vue";
import axios from "axios";
import {useRouter} from "vue-router";

const searchQuery = ref('')
const searchResults = ref(null)
const queryTimeout = ref(null)
const searchError = ref(null)
const router = useRouter()

const mapboxAPIKey =
    "pk.eyJ1Ijoiam9obmtvbWFybmlja2kiLCJhIjoiY2t5NjFzODZvMHJkaDJ1bWx6OGVieGxreSJ9.IpojdT3U3NENknF6_WhR2Q";

const showSearchResult = () => {
  clearTimeout(queryTimeout.value)
  queryTimeout.value = setTimeout(async () => { //This will call after 300 ms
    if (searchQuery.value !== '') {
      try {
        const result = await axios.get(`https://api.mapbox.com/geocoding/v5/mapbox.places/${searchQuery.value}.json?access_token=${mapboxAPIKey}&types=place`)
        searchResults.value = result.data.features
        console.log('search Result', searchResults.value)
      } catch {
        searchError.value = true
      }
      return
    }
    searchResults.value = null
  }, 300)
}

const previewCity = (result) => {
  console.log(result);
  const [city, state] = result.place_name.split(',')

  router.push({
    name: 'cityView',
    params: {
      state: state.replaceAll(' ', ''),
      city: city
    },
    query:{
      lat: result.geometry.coordinates[1],
      lng: result.geometry.coordinates[0],
    }
  })
}
</script>