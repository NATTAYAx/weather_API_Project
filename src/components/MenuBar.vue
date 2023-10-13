<template>
  <button class="my-location menu-btn" @click="handleGetLocationData">
    <svg class="menu-icon" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512">
      <path
        d="M256 0c17.7 0 32 14.3 32 32V66.7C368.4 80.1 431.9 143.6 445.3 224H480c17.7 0 32 14.3 32 32s-14.3 32-32 32H445.3C431.9 368.4 368.4 431.9 288 445.3V480c0 17.7-14.3 32-32 32s-32-14.3-32-32V445.3C143.6 431.9 80.1 368.4 66.7 288H32c-17.7 0-32-14.3-32-32s14.3-32 32-32H66.7C80.1 143.6 143.6 80.1 224 66.7V32c0-17.7 14.3-32 32-32zM128 256a128 128 0 1 0 256 0 128 128 0 1 0 -256 0zm128-80a80 80 0 1 1 0 160 80 80 0 1 1 0-160z" />
    </svg>
  </button>

  <div class="search-location">
    <input id="search" type="text" placeholder="Enter a city name" v-model="city" @keyup.enter="handleGetWeatherData" />
    <label for="search">
      <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512">
        <path
          d="M416 208c0 45.9-14.9 88.3-40 122.7L502.6 457.4c12.5 12.5 12.5 32.8 0 45.3s-32.8 12.5-45.3 0L330.7 376c-34.4 25.2-76.8 40-122.7 40C93.1 416 0 322.9 0 208S93.1 0 208 0S416 93.1 416 208zM208 352a144 144 0 1 0 0-288 144 144 0 1 0 0 288z" />
      </svg>
    </label>
  </div>

  <input type="checkbox" id="favs" />
  <label for="favs" class="fav-locations menu-btn">
    <img class="menu-icon" src="https://www.svgrepo.com/show/13695/star.svg" alt="Favorite Icon" />
  </label>

  <div class="favorites">
    <div class="favorites-title">Favorites</div>
    <div class="favorites-counter" v-if="favorites.length === 0">No favorites selected.</div>
    <div class="favorites-data-cards no-scrollbar" v-else>
      <div class="favorite-data-card" v-for="favorite in favorites" :key="favorite">
        <div class="weather-informations">
          <div class="date w-full text-center text-sm sm:text-base lg:w-1/4 lg:text-right">
            {{ favorite.weatherData.location.localtime }}
          </div>
          <div class="city w-full text-center text-3xl sm:text-4xl lg:w-2/4">
            {{ favorite.city }}
          </div>
          <div class="country w-full text-center text-sm sm:text-base lg:w-1/4">
            {{ favorite.country }}
          </div>

          <button class="fav-btn" @click="removeToFavorites(favorite)">
            <svg class="h-full text-red-600 transition-all duration-300 ease-in-out hover:scale-125"
              xmlns="http://www.w3.org/2000/svg" viewBox="0 0 448 512">
              <path
                d="M135.2 17.7L128 32H32C14.3 32 0 46.3 0 64S14.3 96 32 96H416c17.7 0 32-14.3 32-32s-14.3-32-32-32H320l-7.2-14.3C307.4 6.8 296.3 0 284.2 0H163.8c-12.1 0-23.2 6.8-28.6 17.7zM416 128H32L53.2 467c1.6 25.3 22.6 45 47.9 45H346.9c25.3 0 46.3-19.7 47.9-45L416 128z" />
            </svg>
          </button>
        </div>
        <div class="weather-details">
          <div class="temperature details">
            <div class="details-left">
              <img class="todaydata-icon" src="https://www.svgrepo.com/show/22577/thermometer.svg" alt="Temperature Icon"
                style="width: 35px; height: 35px;" />
            </div>
            <div class="details-right">
              <div class="details-title">Temperature</div>
              <div class="details-content">
                {{ `${favorite.weatherData.current.feelslike_c} °C` }}
              </div>
            </div>
          </div>

          <div class="current-condition details">
            <div class="details-left">
              <img class="h-12" :src="favorite.weatherData.current.condition.icon"
                :alt="favorite.weatherData.current.condition.text" />
            </div>
            <div class="details-right">
              <div class="details-title">Current Condition</div>
              <div class="details-content">
                <div>
                  {{ favorite.weatherData.current.condition.text }}
                </div>
              </div>
            </div>
          </div>

          <div class="wind-speed details">
            <div class="details-left">
              <div class="details-left">
                <img class="todaydata-icon" width="96" height="96" src="https://www.svgrepo.com/show/115278/wind.svg"
                  alt="Wind Speed Icon" />
              </div>
            </div>
            <div class="details-right">
              <div class="details-title">Wind Speed</div>
              <div class="details-content">
                {{ `${favorite.weatherData.current.wind_kph} km/h` }}
              </div>
            </div>
          </div>

          <div class="humidity details">
            <div class="details-left">
              <img class="todaydata-icon" src="https://www.svgrepo.com/show/324070/water-drops.svg" alt="Humidity Icon" style="width:35px; height: 35px; "/>
            </div>
            <div class="details-right">
              <div class="details-title">Humidity</div>
              <div class="details-content">
                {{ `${favorite.weatherData.current.humidity}%` }}
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { useWeatherStore } from '@/stores/WeatherData.js'
import { onMounted, ref, computed } from 'vue'

const city = ref('Chiang mai')
const weatherStore = useWeatherStore()

const handleGetWeatherData = () => {
  weatherStore.getWeatherData(city.value)
  city.value = ''
}

const handleGetLocationData = () => {
  weatherStore.getLocationData()
  city.value = ''
}

onMounted(() => {
  weatherStore.getWeatherData(city.value)
})


const favorites = computed(() => weatherStore.favorites)

const removeToFavorites = (favorite) => {
  weatherStore.removeFavorite(favorite)
}
</script>

<style scoped>
@tailwind utilities;

@layer utilities {
  .menu-btn {
    @apply flex h-full w-16 shrink-0 items-center justify-center bg-gray-800 transition duration-300 ease-in-out;
  }

  .menu-btn:hover {
    @apply scale-110;
  }

  #favs {
    @apply hidden;
  }

  .fav-locations {
    @apply cursor-pointer;
  }

  .menu-icon {
    @apply h-5 text-white;
  }

  .search-location {
    @apply relative flex h-full w-full shrink items-center;
  }

  .search-location input {
    @apply h-full w-full pl-12 pr-4 outline-none;
  }

  .search-location svg {
    @apply absolute left-4 top-1/2 h-5 w-5 -translate-y-1/2 transform hover:cursor-text;
  }

  .favorites {
    @apply absolute -left-6 top-0 z-10 flex h-full w-[83%] -translate-y-[calc(100%+48px)] flex-col items-center gap-10 bg-gray-800 p-6 text-white transition-all duration-300 ease-in-out sm:left-0 sm:rounded-bl-xl sm:rounded-tl-xl sm:p-12;
  }

  .favorites {
    box-shadow: 15px 0px 25px rgba(0, 0, 0, 0.1);
  }

  #favs:checked~.favorites {
    @apply translate-y-0;
  }

  .favorites-title {
    @apply text-4xl;
  }

  .favorites-counter {
    @apply text-xl;
  }

  .favorites-data-cards {
    @apply flex h-full w-full flex-col overflow-y-scroll;
  }

  .favorite-data-card {
    @apply w-full bg-gray-900 p-6;
  }

  .weather-informations {
    @apply relative mb-6 flex flex-col items-center justify-center gap-4 lg:flex-row lg:gap-0;
  }

  .fav-btn {
    @apply absolute right-0 top-0 h-4;
  }

  .weather-details {
    @apply flex flex-wrap;
  }

  .details {
    @apply flex w-full items-center justify-center bg-gray-800 py-4 lg:w-[calc(50%-12px)];
  }

  .details-left {
    @apply flex w-2/6 items-center justify-center;
  }

  .details-icon {
    @apply h-6 lg:h-8;
  }

  .details-right {
    @apply flex w-4/6 flex-col justify-center gap-2;
  }

  .no-scrollbar {
    -ms-overflow-style: none;
    scrollbar-width: none;
  }

  .no-scrollbar::-webkit-scrollbar {
    display: none;
  }

}
</style>
