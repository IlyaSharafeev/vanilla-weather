<template>
  <div class="wrapper">
    <label class="like-switch-label">
      <input @change="switchInput" type="checkbox" class="like-switch" :checked="isChecked" />
      <div class="switch-cover">
        <span class="switch-lang switch-lang--ru">uk</span>
        <span class="switch-lang switch-lang--ua">en</span>
        <div class="switch-circle"></div>
      </div>
    </label>
  </div>
</template>

<script setup>
import {ref} from "vue";

const isChecked = ref(true);

isChecked.value = localStorage.getItem("lang") === "en";

const switchInput = () => {
  if(localStorage.getItem("lang") === "en") {
    localStorage.setItem("lang", "uk");
  } else {
    localStorage.setItem("lang", "en");
  }
}
</script>

<style scoped lang="sass">
// General
body
  font-family: Arial, sans-serif
  font-size: 16px
  background-color: #f6f6f6

*
  box-sizing: border-box

.wrapper
  text-align: center
  border-radius: 16px
  padding: 0 20px

.like-switch-label__text
  display: block
  margin-right: 20px
  font-size: 22px

// Switcher
.like-switch-label
  display: flex
  align-items: center

.like-switch
  position: absolute
  opacity: 0
  &:checked + .switch-cover > .switch-circle
    transform: translate3d(34px, 0, 0)
  &:checked + .switch-cover > .switch-lang--ru
    opacity: 0
  &:checked + .switch-cover > .switch-lang--ua
    opacity: 1

.switch-cover
  width: 68px
  height: 34px
  cursor: pointer
  border-radius: 17px
  background-color: transparent
  transition: background-color .4s
  padding: 6px
  border: 2px solid #dfdfe2
  position: relative

.switch-circle
  width: 18px
  height: 18px
  border-radius: 50%
  background-color: #dfdfe2
  transition: transform 0.4s ease
  pointer-events: none
  position: relative
  z-index: 2

.switch-lang
  position: absolute
  top: 6px
  color: #ffffff
  transition: opacity .2s

.switch-lang--ru
  right: 6px

.switch-lang--ua
  left: 6px
  opacity: 0

</style>
