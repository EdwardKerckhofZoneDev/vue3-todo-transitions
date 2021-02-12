<template>
  <div class="header">
    <div class="container container-header">
      <h1>Todo</h1>
      <button @click="toggleDarkMode">
        <img :src="imageSrc" alt="Dark Mode Toggle" />
      </button>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, onMounted, ref } from 'vue'

export default defineComponent({
  name: 'AppHeader',

  setup() {
    const imageSrc = ref('../src/assets/img/icon-moon.svg')
    let darkMode: boolean = false

    const enableDarkMode = () => {
      imageSrc.value = '../src/assets/img/icon-sun.svg'
      document.body.classList.add('darkmode')
      localStorage.setItem('darkMode', JSON.stringify(true))
    }

    const disableDarkMode = () => {
      imageSrc.value = '../src/assets/img/icon-moon.svg'
      document.body.classList.remove('darkmode')
      localStorage.setItem('darkMode', JSON.stringify(false))
    }

    onMounted(() => {
      let x = localStorage.getItem('darkMode')
      if (x && JSON.parse(x)) {
        enableDarkMode()
      } else {
        disableDarkMode()
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
