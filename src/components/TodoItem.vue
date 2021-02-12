<template>
  <li @click="checkCompleted">
    <div class="todo-checker"></div>
    <p>{{ todo.text }}</p>
    <img
      src="../assets/img/icon-cross.svg"
      alt="Delete Todo"
      class="todo-remover"
      @click="emitClick"
    />
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

  emits: ['clicked', 'checked'],

  setup(props, { emit }) {
    const emitClick = () => {
      emit('clicked', props.todo.id)
    }

    const checkCompleted = () => {
      emit('checked', props.todo.id)
    }

    return { emitClick, checkCompleted }
  }
})
</script>
