<template>
  <div
    class="wrapper"
  >
    <SiteNavigation />
    <RouterView class="router-view" v-slot="{ Component }">
      <Transition name="page">
        <component :is="Component" />
      </Transition>
    </RouterView>
  </div>
</template>

<script setup>
import { RouterView } from "vue-router";
import SiteNavigation from "./components/SiteNavigation.vue";
</script>

<style lang="scss" scoped>
.wrapper {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
  background-color: #02838f;
}

.router-view {
  flex: 1 1 0%;
}
</style>
