<template>
  <input
    v-model="newTodo"
    type="text"
    required
    placeholder="Add a new todo..."
    @keypress.enter="emitAddNewTodo(newTodo.value)"
  />
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue'

export default defineComponent({
  name: 'AddTodo',

  emits: ['newValue', 'badValue'],

  setup(props, { emit }) {
    const newTodo = ref('')

    const emitAddNewTodo = () => {
      if (!newTodo.value) emit('badValue')
      emit('newValue', newTodo.value)
      newTodo.value = ''
    }

    return { newTodo, emitAddNewTodo }
  }
})
</script>

<style lang="scss" scoped></style>
