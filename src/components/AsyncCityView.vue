<template>
  <div class="city-view">
    <!-- Weather Overview -->
    <div class="city-view__weather-overview">
      <h1>{{ route.params.city }}</h1>
      <p class="date">
        {{
          new Date(weatherData.currentTime).toLocaleDateString(
            "en-us",
            {
              weekday: "short",
              day: "2-digit",
              month: "long",
            }
          )
        }}
        {{
          new Date(weatherData.currentTime).toLocaleTimeString(
            "en-us",
            {
              timeStyle: "short",
            }
          )
        }}
      </p>
      <p class="temp">
        {{ Math.round(weatherData.current.temp) }}&deg;
      </p>
      <p>
        Feels like
        {{ Math.round(weatherData.current.feels_like) }} &deg;
      </p>
      <p class="description">
        {{ weatherData.current.weather[0].description }}
      </p>
      <img
        class="image"
        :src="
          `http://openweathermap.org/img/wn/${weatherData.current.weather[0].icon}@2x.png`
        "
        alt="image weather"
      />
    </div>

    <hr />

    <!-- Hourly Weather -->
    <div class="city-view__hourly-weather-wrapper">
      <div class="city-view__hourly-weather">
        <h2>Hourly Weather</h2>
        <div class="weather-block-container">
          <div
            v-for="hourData in weatherData.hourly"
            :key="hourData.dt"
            class="weather-block"
          >
            <p>
              {{
                new Date(
                  hourData.currentTime
                ).toLocaleTimeString("en-us", {
                  hour: "numeric",
                })
              }}
            </p>
            <img
              :src="
                `http://openweathermap.org/img/wn/${hourData.weather[0].icon}@2x.png`
              "
              alt="weather-icon"
            />
            <p>
              {{ Math.round(hourData.temp) }}&deg;
            </p>
          </div>
        </div>
      </div>
    </div>

    <LineChart :weatherData="weatherData"/>

    <hr />

    <!-- Weekly Weather -->
    <div class="city-view__weakly-weather">
      <div>
        <h2>7 Day Forecast</h2>
        <div
          v-for="day in weatherData.daily"
          :key="day.dt"
          class="weather-block"
        >
          <p>
            {{
              new Date(day.dt * 1000).toLocaleDateString(
                "en-us",
                {
                  weekday: "long",
                }
              )
            }}
          </p>
          <img
            :src="
              `http://openweathermap.org/img/wn/${day.weather[0].icon}@2x.png`
            "
            alt=""
          />
          <div class="temperature">
            <p>H: {{ Math.round(day.temp.max) }}</p>
            <p>L: {{ Math.round(day.temp.min) }}</p>
          </div>
        </div>
      </div>
    </div>

  </div>
</template>

<script setup>
import axios from "axios";
import { useRoute } from "vue-router";
import LineChart from "@/components/LineChart.vue";
import {ref} from "vue";

const route = useRoute();
const hourlyTempData = ref(null);

const getWeatherData = async () => {
  try {
    const weatherData = await axios.get(
      `https://api.openweathermap.org/data/2.5/onecall?lat=${route.query.lat}&lon=${route.query.lng}&exclude={part}&appid=7efa332cf48aeb9d2d391a51027f1a71&units=metric&lang=en`
    );

    // cal current date & time
    const localOffset = new Date().getTimezoneOffset() * 60000;
    const utc = weatherData.data.current.dt * 1000 + localOffset;
    weatherData.data.currentTime =
      utc + 1000 * weatherData.data.timezone_offset;

    // cal hourly weather offset
    weatherData.data.hourly.forEach((hour) => {
      const utc = hour.dt * 1000 + localOffset;
      hour.currentTime =
        utc + 1000 * weatherData.data.timezone_offset;
    });

    return weatherData.data;
  } catch (err) {
    console.log(err);
  }
};

const weatherData = await getWeatherData();
</script>

<style lang="scss" scoped>
@import "../assets/media";

.city-view {
  margin: 0 auto;
  width: 100%;
  padding: 0 30px;
  display: flex;
  flex-direction: column;
  flex: 1 1 0%;
  align-items: center;

  @include media_tablet {
    padding: initial;
    width: 768px;
  }

  @include media_desktop {
    width: 1200px;
  }


  &__weather-overview {
    display: flex;
    padding-top: 3rem;
    padding-bottom: 3rem;
    color: #ffffff;
    flex-direction: column;
    align-items: center;

    h1 {
      margin-bottom: 0.5rem;
      font-size: 2.25rem;
      line-height: 2.5rem;
    }

    .date {
      margin-bottom: 3rem;
      font-size: 0.875rem;
      line-height: 1.25rem;
    }

    .temp {
      margin-bottom: 2rem;
      font-size: 6rem;
      line-height: 1;
    }

    .description {
      text-transform: capitalize;
    }

    .image {
      height: auto;
      width: 150px;
    }
  }

  hr {
    width: 100%;
    border-width: 1px;
    border-color: #ffffff;
    border-opacity: 0.1;
  }

  &__hourly-weather-wrapper {
    padding-top: 3rem;
    padding-bottom: 3rem;
    width: 100%;
    max-width: 768px;

    .city-view__hourly-weather {
      margin-left: 2rem;
      margin-right: 2rem;
      color: #ffffff;
    }

    h2 {
      margin-bottom: 1rem;
    }

    .weather-block-container {
      display: flex;
      overflow-x: scroll;
      gap: 2.5rem;

      .weather-block {
        display: flex;
        flex-direction: column;
        align-items: center;
        gap: 1rem;

        p {
          white-space: nowrap;
        }

        img {
          object-fit: cover;
          width: auto;
          height: 50px;
        }
      }
    }
  }

  &__weakly-weather {
    padding-top: 3rem;
    padding-bottom: 3rem;
    width: 100%;
    max-width: 768px;
    color: white;


    h2 {
      margin-bottom: 1rem;
    }

    .weather-block {
      display: flex;
      align-items: center;

      p {
        flex: 1 1 0%;
      }

      img {
        object-fit: cover;
        width: 50px;
        height: 50px;
      }

      .temperature {
        display: flex;
        flex: 1 1 0%;
        justify-content: flex-end;
        gap: 0.5rem;
      }
    }
  }
}
</style>
