<template>
  <toast-message v-if="showToastMessage" />
  <section class="home">
    <app-header />
    <div class="container home-container">
      <todo-list @invalidValue="triggerToastMessage" />
    </div>
  </section>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue'
import ToastMessage from './components/ToastMessage.vue'
import TodoList from './components/TodoList.vue'
import AppHeader from './components/AppHeader.vue'

export default defineComponent({
  name: 'App',

  components: { ToastMessage, TodoList, AppHeader },

  setup() {
    const showToastMessage = ref(false)

    const triggerToastMessage = () => {
      showToastMessage.value = true
      setTimeout(() => (showToastMessage.value = false), 3000)
    }

    const toggleDarkMode = () => {}

    return { showToastMessage, triggerToastMessage, toggleDarkMode }
  }
})
</script>

<style lang="scss">
@import './assets/scss/main.scss';

.home-container {
  margin-top: -8rem;
}
</style>
