<template>
  <header class="header">
    <nav>
      <RouterLink :to="{ name: 'home' }">
        <div class="logo">
          <i class="fa-solid fa-sun"></i>
          <p>The Weather</p>
        </div>
      </RouterLink>

      <div class="add-button">
        <i
          class="fa-solid fa-plus"
          @click="addCity"
          v-if="route.name === 'cityView'"
        ></i>
      </div>


      <BaseModal
          :modalActive="modalActive"
          @close-modal="toggleModal"
      >
        <div class="modal-text">
          <p>
            {{modalText}}
          </p>
        </div>
      </BaseModal>

      <LangSwitcher/>

    </nav>
  </header>
</template>

<script setup>
import { RouterLink, useRoute, useRouter } from "vue-router";
import { uid } from "uid";
import { ref } from "vue";
import BaseModal from "./BaseModal.vue";
import LangSwitcher from "@/components/LangSwitcher.vue";

const savedCities = ref([]);
const route = useRoute();
const router = useRouter();
const modalActive = ref(false);
const modalText = ref("");
const addCity = async () => {
  if (savedCities.value.length >= 5) {
    toggleModal("You cannot add more than 5 cities. Please clean up any unnecessary cities.");
    return;
  }

  if (localStorage.getItem("savedCities")) {
    savedCities.value = JSON.parse(
      localStorage.getItem("savedCities")
    );
  }

  const locationObj = {
    id: uid(),
    state: route.params.state,
    city: route.params.city,
    coords: {
      lat: route.query.lat,
      lng: route.query.lng,
    },
  };


  // Checking if there is such a city in the repository
  let isAlreadyCity = savedCities.value.find((city) => city.city === locationObj.city);
  if(isAlreadyCity) {
    return;
  }

  savedCities.value.push(locationObj);
  localStorage.setItem(
    "savedCities",
    JSON.stringify(savedCities.value)
  );

  let query = Object.assign({}, route.query);
  delete query.preview;
  query.id = locationObj.id;
  router.replace({ query });
  toggleModal("You have successfully added a city");
};

const toggleModal = (text) => {
  modalText.value = text;
  modalActive.value = !modalActive.value;
};
</script>

<style lang="scss" scoped>
@import "../assets/media";

.header {
  margin: 0 auto;
  width: 100%;
  padding: 0 20px;
  box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -2px rgba(0, 0, 0, 0.05);

  @include media_tablet {
    padding: 0 50px;
  }

  @include media_desktop {
    padding: 0 100px;
  }

  nav {
    display: flex;
    padding-top: 1.5rem;
    padding-bottom: 1.5rem;
    color: #ffffff;
    flex-direction: column;
    align-items: center;
    gap: 1rem;

    @media (min-width: 640px) {
      flex-direction: row;
    }

    .logo {
      display: flex;
      align-items: center;
      gap: 0.75rem;
      color: white;
      text-decoration: none;

      i {
        font-size: 1.5rem;
        line-height: 2rem;
      }

      p {
        font-size: 1.5rem;
        line-height: 2rem;
      }
    }

    .add-button {
      display: flex;
      flex: 1 1 0%;
      justify-content: flex-end;
      gap: 0.75rem;

      i {
        transition-duration: 150ms;
        font-size: 1.25rem;
        line-height: 1.75rem;
        cursor: pointer;
      }
    }

    .modal-text {
      color: black;

      p {
        margin-bottom: 1rem;
      }
    }
  }
}
</style>
