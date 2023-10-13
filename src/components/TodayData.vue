<template>
  <div class="today">
    <div class="section-description">Today</div>

    <div class="today-card">
      <div class="weather-informations">
        <div class="date w-full text-center text-sm sm:w-1/4 sm:text-right sm:text-lg md:text-xl">
          {{ isLoading ? 'Loading...' : formattedLocalTime }}
        </div>
        <div class="city w-full text-center text-4xl sm:w-2/4 sm:text-5xl">
          {{ isLoading ? 'Loading...' : weatherData.location.name }}
        </div>
        <div class="country w-full text-center text-sm sm:w-1/4 sm:text-lg md:text-xl">
          {{ isLoading ? 'Loading...' : weatherData.location.country }}
        </div>
        <button class="fav-btn" @click="addToFavorites" v-if="!isFavorite">
          <img class="h-full" src="https://www.svgrepo.com/show/13695/star.svg" alt="Favorite Icon" />
        </button>
      </div>
      <div class="weather-details">
        <div class="temperature details">
          <div class="details-left">
            <img class="todaydata-icon"
              src="https://www.svgrepo.com/show/22577/thermometer.svg"
              alt="Temperature Icon" />
          </div>
          <div class="details-right">
            <div class="details-title">Temperature</div>
            <div class="details-content">
              {{ isLoading ? 'Loading...' : `${weatherData.current.feelslike_c} °C` }}
            </div>
          </div>
        </div>
        <div class="current-condition details">
          <div class="details-left">
            <img class="h-12" :src="isLoading ? '' : weatherData.current.condition.icon"
              :alt="isLoading ? '' : weatherData.current.condition.text" />
          </div>
          <div class="details-right">
            <div class="details-title">Current Condition</div>
            <div class="details-content">
              <div>
                {{ isLoading ? 'Loading...' : weatherData.current.condition.text }}
              </div>
            </div>
          </div>
        </div>

        <div class="wind-speed details">
          <div class="details-left">
            <img class="todaydata-icon" width="96" height="96" src="https://www.svgrepo.com/show/115278/wind.svg"
              alt="Wind Speed Icon" />
          </div>
          <div class="details-right">
            <div class="details-title">Wind Speed</div>
            <div class="details-content">
              {{ isLoading ? 'Loading...' : `${weatherData.current.wind_kph} km/h` }}
            </div>
          </div>
        </div>

        <div class="humidity details">
          <div class="details-left">
            <img class="todaydata-icon" width="96" height="96" src="https://www.svgrepo.com/show/324070/water-drops.svg"
              alt="Humidity Icon" />
          </div>
          <div class="details-right">
            <div class="details-title">Humidity</div>
            <div class="details-content">
              {{ isLoading ? 'Loading...' : `${weatherData.current.humidity}%` }}
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { useWeatherStore } from '@/stores/WeatherData.js'
import { computed } from 'vue'
import { storeToRefs } from 'pinia'

const { weatherData, isLoading, favorites } = storeToRefs(useWeatherStore())

const formattedLocalTime = computed(() => {
  const localTime = new Date(weatherData.value.location.localtime)

  const options = {
    day: 'numeric',
    month: 'long',
    weekday: 'long'
  }

  const formatter = new Intl.DateTimeFormat('en-US', options)
  const formattedDate = formatter.format(localTime)

  const parts = formattedDate.split(', ')
  const weekday = parts[0]
  const month = parts[1]

  return `${month} - ${weekday}`
})

const weatherStore = useWeatherStore()

const addToFavorites = () => {
  const { name: city, country } = weatherData.value.location
  const favorite = { city, country, weatherData: weatherData.value }
  weatherStore.addToFavorite(favorite)
}

const isFavorite = computed(() => {
  const cityName = weatherData.value?.location?.name
  return favorites.value.some((favorite) => favorite.city === cityName)
})
</script>

<style scoped>
@tailwind utilities;

@layer utilities {
  .today {
    @apply mb-12;
  }

  .today-card {
    @apply flex flex-col;
  }

  .weather-informations {
    @apply relative flex flex-col items-center justify-center gap-5 bg-gray-800 p-4 text-white sm:h-40 sm:flex-row sm:gap-0;
  }

  .fav-btn {
    @apply absolute right-6 top-6 h-5;
  }

  .weather-details {
    @apply flex flex-wrap;
  }

  .details {
    @apply flex h-[120px] w-full items-center justify-center bg-gray-800 py-4 sm:w-[calc(50%-12px)];
  }

  .details-left {
    @apply flex w-1/2 items-center justify-center sm:w-1/3;
  }

  .todaydata-icon {
    @apply h-8 text-white;
  }

  .details-right {
    @apply flex w-1/2 flex-col justify-center gap-2 sm:w-2/3;
  }

  .details-title {
    @apply font-medium;
  }

  .details-content {
    @apply text-xl sm:text-2xl md:text-3xl;
  }
}
</style>
