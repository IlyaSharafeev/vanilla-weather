<template>
  <div class="city-skeleton">
    <div class="city-skeleton-overview">
      <AnimatedPlaceholder class="first-placeholder" />
      <AnimatedPlaceholder class="second-placeholder" />
      <AnimatedPlaceholder
        class="third-placeholder"
      />
      <AnimatedPlaceholder class="fourth-placeholder" />
      <AnimatedPlaceholder
        class="fifth-placeholder"
      />
    </div>
    <div class="city-skeleton-hourly">
      <AnimatedPlaceholder
        class="first-placeholder"
      />
    </div>
    <div class="city-skeleton-weekly">
      <AnimatedPlaceholder
        class="first-placeholder"
      />
    </div>
  </div>
</template>

<script setup>
import AnimatedPlaceholder from "./AnimatedPlaceholder.vue";
</script>

<style scoped lang="scss">
.city-skeleton {
  display: flex;
  flex-direction: column;
  flex: 1 1 0%;

  &-overview {
    display: flex;
    padding-top: 3rem;
    padding-bottom: 3rem;
    flex-direction: column;
    align-items: center;

    .first-placeholder {
      max-width: 300px;
      width: 100%;
      margin-bottom: 0.5rem;
    }

    .second-placeholder {
      max-width: 300px;
      width: 100%;
      margin-bottom: 3rem;
    }

    .third-placeholder {
      max-width: 300px;
      margin-bottom: 3rem;
      width: 100%;
      height: 100px;
    }

    .fourth-placeholder {
      max-width: 300px;
      width: 100%;
      margin-bottom: 2rem;
    }

    .fifth-placeholder {
      max-width: 300px;
      width: 100%;
      height: 75px;
    }
  }

  &-hourly {
    display: flex;
    padding: 3rem 2rem;
    flex-direction: column;
    align-items: center;

    .first-placeholder {
      margin-bottom: 3rem;
      width: 100%;
      max-width: 768px;
      height: 100px;
    }
  }

  &-weekly {
    display: flex;
    padding: 3rem 2rem;
    flex-direction: column;
    align-items: center;

    .first-placeholder {
      margin-bottom: 3rem;
      width: 100%;
      max-width: 768px;
      height: 100px;
    }
  }
}
</style>

