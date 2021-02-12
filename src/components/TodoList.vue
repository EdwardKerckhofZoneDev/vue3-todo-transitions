<template>
  <div class="todo-list-container">
    <add-todo @newValue="addNewTodo" @badValue="emitBadValue" />
    <ul v-if="todos.length > 0" class="todo-list">
      <todo-item
        v-for="todo in todos"
        :key="todo.id"
        :todo="todo"
        @clicked="deleteTodo"
      />
      <li class="todo-list-item todo-list-info">
        <p>{{ todos.length }} items left</p>
        <div class="filters">
          <button class="filter-active">All</button>
          <button>Active</button>
          <button>Completed</button>
        </div>
        <button class="clear">Clear Completed</button>
      </li>
    </ul>
    <p v-else class="todo-list-item list-empty">Nothing left to do!</p>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue'
import TodoItem from './TodoItem.vue'
import AddTodo from './AddTodo.vue'

export default defineComponent({
  name: 'TodoList',

  components: { TodoItem, AddTodo },

  emits: ['invalidValue'],

  setup(props, { emit }) {
    const todos = ref([
      { text: 'Streviz', id: 1 },
      { text: 'Homework', id: 2 }
    ])

    const addNewTodo = (newTodo: string) => {
      todos.value.push({ text: newTodo, id: Math.random() })
      newTodo = ''
    }

    const deleteTodo = (id: number) => {
      todos.value = todos.value.filter((todo) => todo.id !== id)
    }

    const emitBadValue = () => {
      emit('invalidValue')
    }

    return { todos, addNewTodo, deleteTodo, emitBadValue }
  }
})
</script>
