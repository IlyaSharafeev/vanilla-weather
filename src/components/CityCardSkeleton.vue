<template>
  <div
    class="skeleton-loader"
  >
    <div class="block-first">
      <AnimatedPlaceholder class="placeholder-first" />
      <AnimatedPlaceholder class="placeholder-second" />
    </div>

    <div class="block-second">
      <AnimatedPlaceholder class="placeholder-first" />
      <AnimatedPlaceholder class="placeholder-second" />
    </div>
  </div>
</template>

<script setup>
import AnimatedPlaceholder from "./AnimatedPlaceholder.vue";
</script>

<style lang="scss" scoped>
.skeleton-loader {
  display: flex;
  padding: 1.5rem 0.75rem;
  border-radius: 0.375rem;
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06);

  .block-first {
    display: flex;
    flex-direction: column;
    flex: 1 1 0%;
    gap: 0.5rem;

    .placeholder-first {
      max-width: 50%;
    }

    .placeholder-second {
      max-width: 40%;
    }
  }

  .block-second {
    display: flex;
    flex-direction: column;
    flex: 1 1 0%;
    align-items: flex-end;
    gap: 0.5rem;

    .placeholder-first {
      max-width: 50px;
      width: 100%;
    }

    .placeholder-second {
      max-width: 75px;
      width: 100%;
    }
  }
}
</style>
