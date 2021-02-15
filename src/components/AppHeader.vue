<template>
  <div class="header">
    <div class="container container-header">
      <h1>Todo</h1>
      <button @click="toggleDarkMode">
        <img :src="imageSrc" alt="Dark Mode Toggle" />
      </button>
    </div>
    <div class="container">
      <p>Your own simple ToDo list!</p>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, onMounted, ref } from 'vue'
import iconMoon from '../assets/img/icon-moon.svg'
import iconSun from '../assets/img/icon-sun.svg'
export default defineComponent({
  name: 'AppHeader',

  setup() {
    const imageSrc = ref(iconMoon)
    let darkMode: boolean = false

    const enableDarkMode = () => {
      imageSrc.value = iconSun
      document.body.classList.add('darkmode')
      localStorage.setItem('darkMode', JSON.stringify(true))
    }

    const disableDarkMode = () => {
      imageSrc.value = iconMoon
      document.body.classList.remove('darkmode')
      localStorage.setItem('darkMode', JSON.stringify(false))
    }

    onMounted(() => {
      let x = localStorage.getItem('darkMode')
      if (x && JSON.parse(x)) {
        enableDarkMode()
      }
    })

    const toggleDarkMode = () => {
      darkMode = JSON.parse(localStorage.getItem('darkMode')!)

      if (!darkMode) {
        enableDarkMode()
      } else {
        disableDarkMode()
      }
    }

    return { toggleDarkMode, imageSrc }
  }
})
</script>
