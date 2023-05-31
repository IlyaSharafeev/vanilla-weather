<template>
  <popup-modal ref="popup">
    <p class="description">{{ message }}</p>
    <div class="btns">
      <button class="cancel-btn" @click.stop="_cancel">{{ cancelButton }}</button>
      <span class="ok-btn" @click.stop="_confirm">{{ okButton }}</span>
    </div>
  </popup-modal>
</template>


<script>
import PopupModal from "@/components/PopupModal.vue";

export default {
  name: 'ConfirmDialogue',

  components: { PopupModal },

  data: () => ({
    // Parameters that change depending on the type of dialogue
    title: undefined,
    message: undefined, // Main text content
    okButton: undefined, // Text for confirm button; leave it empty because we don't know what we're using it for
    cancelButton: 'Go Back', // text for cancel button

    // Private variables
    resolvePromise: undefined,
    rejectPromise: undefined,
  }),

  methods: {
    show(opts = {}) {
      this.title = opts.title
      this.message = opts.message
      this.okButton = opts.okButton
      if (opts.cancelButton) {
        this.cancelButton = opts.cancelButton
      }
      // Once we set our config, we tell the popup modal to open
      this.$refs.popup.open()
      // Return promise so the caller can get results
      return new Promise((resolve, reject) => {
        this.resolvePromise = resolve
        this.rejectPromise = reject
      })
    },

    _confirm() {
      this.$refs.popup.close()
      this.resolvePromise(true)
    },

    _cancel() {
      this.$refs.popup.close()
      this.resolvePromise(false)
      // Or you can throw an error
      // this.rejectPromise(new Error('User canceled the dialogue'))
    },
  },
}
</script>

<!-- components/ConfirmDialogue.vue -->

<style scoped>
.description {
  color: black;
  text-align: center;
  padding: 60px 40px;
}

.btns {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
}

.ok-btn {
  padding: 0.5em 1em;
  background-color: #ff9b9b;
  color: #ffffff;
  border: 2px solid #ff7f7f;
  border-radius: 5px;
  font-weight: bold;
  font-size: 16px;
  text-transform: uppercase;
  cursor: pointer;
}

.cancel-btn {
  padding: 0.5em 1em;
  background-color: #d5eae7;
  color: #35907f;
  border: 2px solid #0ec5a4;
  border-radius: 5px;
  font-weight: bold;
  font-size: 16px;
  text-transform: uppercase;
  cursor: pointer;
}
</style>
