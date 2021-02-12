<template>
  <div class="header">
    <div class="container container-header">
      <h1>Todo</h1>
      <button @click="toggleDarkMode">
        <img src="../assets/img/icon-moon.svg" alt="Dark Mode Toggle" />
      </button>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue'

export default defineComponent({
  name: 'AppHeader',

  setup() {
    const toggleDarkMode = () => {}

    return { toggleDarkMode }
  }
})
</script>
