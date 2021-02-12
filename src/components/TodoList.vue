<template>
  <div class="todo-list-container">
    <add-todo @newValue="addNewTodo" @badValue="emitBadValue" />
    <ul v-if="todos.length > 0" class="todo-list">
      <todo-item
        v-for="todo in todos"
        :key="todo.id"
        :todo="todo"
        class="todo-list-item"
        :class="{ completed: todo.completed }"
        @clicked="deleteTodo"
        @checked="checkTodo"
      />
      <li class="todo-list-item todo-list-info">
        <p>{{ todos.length }} item(s) left</p>
        <div class="filters">
          <button class="filter-active" @click="filterAll($event)">All</button>
          <button @click="filterCompleted($event)">Completed</button>
        </div>
        <button class="clear" @click="clearCompleted">Clear Completed</button>
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
      { text: 'Streviz', completed: false, id: 1 },
      { text: 'Homework', completed: false, id: 2 }
    ])

    let todosCopy = [...todos.value]

    const addNewTodo = (newTodo: string) => {
      if (newTodo) {
        todos.value.push({ text: newTodo, completed: false, id: Math.random() })
        newTodo = ''
      }
      updateTodosCopy()
    }

    const deleteTodo = (id: number) => {
      todos.value = todos.value.filter((todo) => todo.id !== id)
      updateTodosCopy()
    }

    const emitBadValue = () => {
      emit('invalidValue')
    }

    const checkTodo = (id: number) => {
      let todo = todos.value.find((todo) => todo.id === id)
      if (todo) todo.completed = !todo.completed
    }

    const clearCompleted = () => {
      todos.value = todos.value.filter((todo) => !todo.completed)
      updateTodosCopy()
    }

    const filterCompleted = ($event: MouseEvent) => {
      todos.value = []
      todosCopy.forEach((todo) => {
        if (todo.completed) todos.value.push(todo)
      })
      if (todos.value.length === 0) todos.value = todosCopy
      else removeActiveClass($event)
    }

    const filterAll = ($event: MouseEvent) => {
      todos.value = todosCopy
      removeActiveClass($event)
    }

    const removeActiveClass = (event: MouseEvent) => {
      const filters = document.querySelector('.filters')
      if (filters) {
        const buttons = filters.querySelectorAll('button')

        if (buttons) {
          buttons.forEach((button) => button.classList.remove('filter-active'))
        }
      }

      ;(event.target as HTMLButtonElement).classList.add('filter-active')
    }

    const updateTodosCopy = () => {
      todosCopy = [...todos.value]
    }

    return {
      todos,
      addNewTodo,
      deleteTodo,
      emitBadValue,
      checkTodo,
      clearCompleted,
      filterCompleted,
      filterAll
    }
  }
})
</script>
