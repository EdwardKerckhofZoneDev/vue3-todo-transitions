<template>
  <input
    v-model="searchTodo"
    type="text"
    required
    placeholder="Search a todo..."
    @keyup="emitSearchTodo"
  />
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue'

export default defineComponent({
  name: 'SearchTodo',

  emits: ['search'],

  setup(props, { emit }) {
    const searchTodo = ref('')

    const emitSearchTodo = () => {
      emit('search', searchTodo.value)
    }

    return { searchTodo, emitSearchTodo }
  }
})
</script>

<style lang="scss" scoped></style>
