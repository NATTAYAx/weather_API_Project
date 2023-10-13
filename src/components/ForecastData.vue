<template>
  <div class="next-days">
    <div class="section-description">Tomorrow</div>

    <div class="next-days-cards">
      <div class="day-card">
        <div class="date">
          {{ isLoading ? 'Loading...' : formattedLocalTime(weatherData[1].date) }}
        </div>

        <div class="informations-section">
          <div class="details">
            <div class="temperature details-section">
              <img class="todaydata-icon" src="https://www.svgrepo.com/show/22577/thermometer.svg" alt="Temperature Icon"
                style="width: 35px; height: 35px;" />
              <div>
                <div class="details-title">Temperature</div>
                <div class="details-content">
                  {{ isLoading ? 'Loading...' : `${weatherData[1].day.avgtemp_c} °C` }}
                </div>
              </div>
            </div>

            <div class="wind-speed details-section">
              <img class="todaydata-icon" src="https://www.svgrepo.com/show/115278/wind.svg" alt="Wind Speed Icon"
                style=" width: 35px; height: 35px; " />
              <div>
                <div class="details-title">Wind Speed</div>
                <div class="details-content">
                  {{ isLoading ? 'Loading...' : `${weatherData[1].day.avgvis_km} km/h` }}
                </div>
              </div>
            </div>
          </div>

          <div class="details">
            <div class="current-condition details-section">
              <img class="details-img" :src="isLoading ? '' : weatherData[1].day.condition.icon"
                :alt="isLoading ? '' : weatherData[1].day.condition.text" />
              <div class="-translate-x-3">
                <div class="details-title">Current Condition</div>
                <div class="details-content">
                  {{ isLoading ? 'Loading...' : weatherData[1].day.condition.text }}
                </div>
              </div>
            </div>

            <div class="humidity details-section">
              <img class="todaydata-icon" src="https://www.svgrepo.com/show/324070/water-drops.svg" alt="Humidity Icon"
                style="width: 35px; height: 35px" />
              <div>
                <div class="details-title">Humidity</div>
                <div class="details-content">
                  {{ isLoading ? 'Loading...' : `${weatherData[1].day.avghumidity}%` }}
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="day-card">
        <div class="date">
          {{ isLoading ? 'Loading...' : formattedLocalTime(weatherData[2].date) }}
        </div>
        <div class="informations-section">
          <div class="details">
            <div class="temperature details-section">
              <img class="todaydata-icon" src="https://www.svgrepo.com/show/22577/thermometer.svg" alt="Temperature Icon"
                style="width: 35px; height: 35px;" />
              <div>
                <div class="details-title">Temperature</div>
                <div class="details-content">
                  {{ isLoading ? 'Loading...' : `${weatherData[2].day.avgtemp_c} °C` }}
                </div>
              </div>
            </div>
            <div class="wind-speed details-section">
              <img class="todaydata-icon" src="https://www.svgrepo.com/show/115278/wind.svg" alt="Wind Speed Icon"
                style=" width: 35px; height: 35px; " />
              <div>
                <div class="details-title">Wind Speed</div>
                <div class="details-content">
                  {{ isLoading ? 'Loading...' : `${weatherData[2].day.avgvis_km} km/h` }}
                </div>
              </div>
            </div>
          </div>
          <div class="details">
            <div class="current-condition details-section">
              <img class="details-img" :src="isLoading ? '' : weatherData[2].day.condition.icon"
                :alt="isLoading ? '' : weatherData[2].day.condition.text" />
              <div class="-translate-x-3">
                <div class="details-title">Current Condition</div>
                <div class="details-content">
                  {{ isLoading ? 'Loading...' : weatherData[2].day.condition.text }}
                </div>
              </div>
            </div>
            <div class="humidity details-section">
              <img class="todaydata-icon" src="https://www.svgrepo.com/show/324070/water-drops.svg" alt="Humidity Icon"
                style="width: 35px; height: 35px" />
              <div>
                <div class="details-title">Humidity</div>
                <div class="details-content">
                  {{ isLoading ? 'Loading...' : `${weatherData[2].day.avghumidity}%` }}
                </div>
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
import { storeToRefs } from 'pinia'
import { computed } from 'vue'

const { isLoading } = storeToRefs(useWeatherStore())

const weatherStore = useWeatherStore()
const weatherData = computed(() => weatherStore.weatherData?.forecast.forecastday)

const formattedLocalTime = (date) => {
  const localTime = new Date(date)

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
}
</script>

<style scoped>
@tailwind utilities;

@layer utilities {
  .next-days-cards {
    @apply flex flex-wrap justify-center;
  }

  /* w-[calc(50%-12px)] */

  .day-card {
    @apply flex w-full min-w-[250px] flex-col gap-3 bg-gray-800 px-6 py-4 sm:w-[calc(50%-12px)] lg:h-[200px];
  }

  .date {
    @apply mb-2 text-center text-white;
  }

  .informations-section {
    @apply flex h-full flex-row flex-wrap sm:flex-col sm:flex-nowrap sm:gap-4 lg:flex-row lg:gap-0;
  }

  .details {
    @apply flex w-1/2 flex-col justify-between gap-4 sm:w-full lg:w-1/2;
  }

  .details-section {
    @apply flex h-[calc(100%-8px)] items-center gap-5;
  }

  .details-img {
    @apply h-10 -translate-x-[6px];
  }

  .forecast-icon {
    @apply h-6 text-white;
  }
}
</style>
