<template>
  <Teleport to="body">
    <Transition name="modal-outer">
      <div
        v-show="modalActive"
        class="base-modal"
      >
        <Transition name="modal-inner">
          <div
            v-if="modalActive"
          >
            <slot />
            <button
              @click="$emit('close-modal')"
            >
              Close
            </button>
          </div>
        </Transition>
      </div>
    </Transition>
  </Teleport>
</template>

<script setup>
defineEmits(["close-modal"]);
defineProps({
  modalActive: {
    type: Boolean,
    default: false,
  },
});
</script>

<style lang="scss" scoped>
.base-modal {
  display: flex;
  position: absolute;
  top: 0;
  left: 0;
  padding-left: 2rem;
  padding-right: 2rem;
  background-color: rgba(0, 0, 0, 0.7);
  --bg-opacity: 0.3;
  justify-content: center;
  width: 100%;
  height: 100vh;

  div {
    padding: 1rem;
    margin-top: 8rem;
    background-color: #ffffff;
    align-self: flex-start;
    max-width: 768px;
  }

  button {
    padding: 0.5rem 1.5rem;
    margin-top: 2rem;
    color: #ffe5e5;
    background-color: #06bdc4;
  }
}
</style>
