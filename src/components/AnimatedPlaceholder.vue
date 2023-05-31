<template>
  <div class="placeholder">
    &nbsp;
  </div>
</template>

<style scoped lang="scss">
.placeholder {
  background-image: linear-gradient(to right, var(--tw-gradient-stops));
  background-color: #F3F4F6;
  animation: pulse 2s cubic-bezier(0.4, 0, 0.6, 1) infinite;

  @keyframes pulse {
    0%, 100% {
      opacity: 1;
    }
    50% {
      opacity: .5;
    }
  };
}
</style>
