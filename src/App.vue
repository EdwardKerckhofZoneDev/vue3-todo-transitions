<template>
  <section class="home">
    <toast-message v-if="showToastMessage" />
    <todo-list @invalidValue="triggerToastMessage" />
  </section>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue'
import ToastMessage from './components/ToastMessage.vue'
import TodoList from './components/TodoList.vue'

export default defineComponent({
  name: 'App',

  components: { ToastMessage, TodoList },

  setup() {
    const showToastMessage = ref(false)

    const triggerToastMessage = () => {
      showToastMessage.value = true
      setTimeout(() => (showToastMessage.value = false), 2500)
    }

    return { showToastMessage, triggerToastMessage }
  }
})
</script>

<style lang="scss">
@import './assets/scss/main.scss';
</style>
