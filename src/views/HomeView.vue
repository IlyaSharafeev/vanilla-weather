<template>
  <main class="home-view">
    <div class="home-view__search">
      <input
        type="text"
        v-model="searchQuery"
        @input="getSearchResults"
        placeholder="Search for a city or state"
      />
      <ul
        v-if="mapboxSearchResults"
      >
        <p class="error" v-if="searchError">
          Sorry, something went wrong, please try again.
        </p>
        <p
          class="error"
          v-if="!searchError && mapboxSearchResults.length === 0"
        >
          No results match your query, try a different term.
        </p>
        <template v-else>
          <li
            v-for="searchResult in mapboxSearchResults"
            :key="searchResult.id"
            class="success"
            @click="previewCity(searchResult)"
          >
            {{ searchResult.place_name }}
          </li>
        </template>
      </ul>
    </div>
    <div class="downloaded">
      <Suspense>
        <CityList />
        <template #fallback>
          <CityCardSkeleton />
        </template>
      </Suspense>
    </div>
  </main>
</template>

<script setup>
import { ref } from "vue";
import axios from "axios";
import { useRouter } from "vue-router";
import CityCardSkeleton from "../components/CityCardSkeleton.vue";
import CityList from "../components/CityList.vue";

const router = useRouter();
const previewCity = (searchResult) => {
  const [city, state] = searchResult.place_name.split(",");
  router.push({
    name: "cityView",
    params: { state: state.replaceAll(" ", ""), city: city },
    query: {
      lat: searchResult.geometry.coordinates[1],
      lng: searchResult.geometry.coordinates[0],
      preview: true,
    },
  });
};

const mapboxAPIKey =
  "pk.eyJ1Ijoiam9obmtvbWFybmlja2kiLCJhIjoiY2t5NjFzODZvMHJkaDJ1bWx6OGVieGxreSJ9.IpojdT3U3NENknF6_WhR2Q";
const searchQuery = ref("");
const queryTimeout = ref(null);
const mapboxSearchResults = ref(null);
const searchError = ref(null);

const getSearchResults = () => {
  clearTimeout(queryTimeout.value);
  queryTimeout.value = setTimeout(async () => {
    if (searchQuery.value !== "") {
      try {
        const result = await axios.get(
          `https://api.mapbox.com/geocoding/v5/mapbox.places/${searchQuery.value}.json?access_token=${mapboxAPIKey}&types=place`
        );
        mapboxSearchResults.value = result.data.features;
      } catch {
        searchError.value = true;
      }

      return;
    }
    mapboxSearchResults.value = null;
  }, 300);
};
</script>

<style lang="scss" scoped>
@import "../assets/media";

.home-view {
  margin: 0 auto;
  width: 100%;
  padding: 0 30px;
  color: white;

  @include media_tablet {
    padding: initial;
    width: 768px;
  }

  @include media_desktop {
    width: 1200px;
  }

  &__search {
    position: relative;
    padding-top: 3rem;
    margin-bottom: 5rem;

    input {
      padding: 0.8rem 1.25rem;
      width: 100%;
      color: blacks;
      border: none;
      border-radius: 5px;

      &:focus {
        border: none;
        outline: none;
      }
    }

    ul {
      position: absolute;
      padding: 0.5rem 0.25rem;
      color: #ffffff;
      width: 100%;
      box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06);
      top: 86px;
      background-color: #2f365b;

      .error {
        padding-top: 0.5rem;
        padding-bottom: 0.5rem;
      }

      .success {
        padding-top: 0.5rem;
        padding-bottom: 0.5rem;
        cursor: pointer;
      }
    }
  }

  .downloaded {
    display: flex;
    flex-direction: column;
    gap: 1rem;
  }
}
</style>
