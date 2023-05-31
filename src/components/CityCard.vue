<template>
  <div
      @click="goToCityView(city)"
      class="city-card"
  >
    <div class="title">
      <h2>{{ city.city }}</h2>
      <h3>{{ city.state }}</h3>
    </div>

    <div class="temperature">
      <div class="temperature-info">
        {{ Math.round(city.weather.main.temp) }}&deg;
      </div>
      <div
          class="trash"
          @click.stop="removeCity"
      >
        <i class="fa-solid fa-trash"></i>
      </div>
    </div>
    <confirm-dialogue ref="confirmDialogue"></confirm-dialogue>
  </div>
</template>

<script setup>
import {useRouter} from "vue-router";
import ConfirmDialogue from "@/components/ConfirmModal.vue";
import {ref} from "vue";

const router = useRouter();
const confirmDialogue = ref(null);

const props = defineProps({
  city: {
    type: Object,
    default: () => {
    },
  },
});

const goToCityView = (city) => {
  router.push({
    name: "cityView",
    params: {state: city.state, city: city.city},
    query: {
      id: city.id,
      lat: city.coords.lat,
      lng: city.coords.lng,
    },
  });
};
const removeCity = async () => {
  const ok = await confirmDialogue.value.show({
    title: 'Delete Page',
    message: 'Are you sure you want to delete this page? It cannot be undone.',
    okButton: 'Delete Forever',
  })
  // If you throw an error, the method will terminate here unless you surround it wil try/catch
  if (ok) {
    const cities = JSON.parse(localStorage.getItem("savedCities"));
    const updatedCities = cities.filter(
        (city) => city.id !== props.city.id
    );
    localStorage.setItem(
        "savedCities",
        JSON.stringify(updatedCities)
    );

    location.reload();
  }
};
</script>

<style lang="scss" scoped>
.city-card {
  display: flex;
  padding: 1.5rem 0.75rem;
  border-radius: 0.375rem;
  cursor: pointer;
  background-color: #484e77;
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06);

  .title {
    display: flex;
    flex-direction: column;
    flex: 1 1 0%;

    h2 {
      font-size: 1.875rem;
      line-height: 2.25rem;
    }
  }

  .temperature {
    display: flex;
    gap: 0.5rem;
    align-items: center;

    .temperature-info {
      font-size: 1.875rem;
      line-height: 2.25rem;
    }

    .trash {
      display: flex;
      transition-duration: 150ms;
      color: #ffffff;
      align-items: center;
      cursor: pointer;
      gap: 0.5rem;
      padding-left: 20px;
      padding-right: 20px;

      :hover {
        color: #EF4444;
      }
    }
  }
}
</style>
