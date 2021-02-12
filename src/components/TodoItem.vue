<template>
  <li class="todo-list-item" @click="emitClick">
    {{ todo.text }}
  </li>
</template>

<script lang="ts">
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'TodoListItem',

  props: {
    todo: {
      type: Object,
      required: true
    }
  },

  emits: ['clicked'],

  setup(props, { emit }) {
    const emitClick = () => {
      emit('clicked', props.todo.id)
    }

    return { emitClick }
  }
})
</script>
