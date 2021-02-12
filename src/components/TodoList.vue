<template>
  <div class="todo-list-container">
    <!-- <div class="inputs"> -->
    <add-todo @newValue="addNewTodo" @badValue="emitBadValue" />
    <!-- <search-todo @search="searchTodo" /> -->
    <!-- </div> -->
    <transition name="switch" mode="out-in">
      <div v-if="todos.length > 0">
        <draggable
          :list="todos"
          :disabled="false"
          ghost-class="ghost"
          tag="transition-group"
          class="todo-list"
          item-key="id"
          :component-data="{ name: 'list', appear: true, tag: 'ul' }"
          @end="endDragging"
        >
          <template #item="{ element }">
            <li
              class="todo-list-item"
              :class="{ completed: element.completed }"
            >
              <div class="todo-checker" @click="checkTodo(element.id)"></div>
              <p @click="checkTodo(element.id)">{{ element.text }}</p>
              <div @click="checkTodo(element.id)" class="empty"></div>
              <img
                src="../assets/img/icon-cross.svg"
                alt="Delete Todo"
                class="todo-remover"
                @click="deleteTodo(element.id)"
              />
            </li>
          </template>
        </draggable>
        <div class="todo-list-item todo-list-info">
          <p>{{ todos.length }} item(s) left</p>
          <div class="filters">
            <button class="filter-active" @click="filterAll($event)">
              All
            </button>
            <button @click="filterCompleted($event)">Completed</button>
          </div>
          <button class="clear" @click="clearCompleted">Clear Completed</button>
        </div>
      </div>
      <p v-else class="todo-list-item list-empty no-pointer">No todo's!</p>
    </transition>
  </div>
</template>

<script lang="ts">
import { defineComponent, onMounted, ref } from 'vue'
import draggable from 'vuedraggable'
import AddTodo from './AddTodo.vue'
import SearchTodo from './SearchTodo.vue'

export default defineComponent({
  name: 'TodoList',

  components: { AddTodo, SearchTodo, draggable },

  emits: ['invalidValue'],

  setup(props, { emit }) {
    const todos = ref<
      {
        text: string
        completed: boolean
        id: number
      }[]
    >([])

    let todosCopy: {
      text: string
      completed: boolean
      id: number
    }[]

    const addNewTodo = (newTodo: string) => {
      if (newTodo) {
        todos.value = [
          { text: newTodo, completed: false, id: Math.random() },
          ...todos.value
        ]
        localStorage.setItem('todos', JSON.stringify(todos.value))
        newTodo = ''
      }
    }

    const deleteTodo = (id: number) => {
      todos.value = todos.value.filter((todo) => todo.id !== id)
      localStorage.setItem('todos', JSON.stringify(todos.value))
    }

    const emitBadValue = () => {
      emit('invalidValue')
    }

    const checkTodo = (id: number) => {
      let todo = todos.value.find((todo) => todo.id === id)
      if (todo) {
        todo.completed = !todo.completed
        localStorage.setItem('todos', JSON.stringify(todos.value))
      }
    }

    const clearCompleted = () => {
      todos.value = todos.value.filter((todo) => !todo.completed)
      localStorage.setItem('todos', JSON.stringify(todos.value))
    }

    const filterCompleted = ($event: MouseEvent) => {
      todos.value = []
      todosCopy.forEach((todo) => {
        if (todo.completed) todos.value.push(todo)
      })
      if (todos.value.length === 0) todos.value = todosCopy
      else removeActiveClass($event)
    }

    const filterAll = ($event?: MouseEvent) => {
      todos.value = todosCopy
      if ($event) {
        removeActiveClass($event)
      }
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

    const searchTodo = (searchValue: string) => {
      todos.value = []
      todosCopy.forEach((todo) => {
        if (todo.text.toLowerCase().includes(searchValue.toLowerCase()))
          todos.value.push(todo)
      })
    }

    const updateTodosCopy = () => {
      todosCopy = [...todos.value]
    }

    const endDragging = () => {
      localStorage.setItem('todos', JSON.stringify(todos.value))
    }

    onMounted(() => {
      let todosFromStorage
      if (localStorage.getItem('todos')) {
        todosFromStorage = JSON.parse(localStorage.getItem('todos')!)
        todos.value = todosFromStorage
      }
      updateTodosCopy()
    })

    return {
      todos,
      addNewTodo,
      searchTodo,
      deleteTodo,
      emitBadValue,
      checkTodo,
      clearCompleted,
      filterCompleted,
      filterAll,
      endDragging
    }
  }
})
</script>
