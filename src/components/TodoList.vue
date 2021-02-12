<template>
  <div class="todo-list-container">
    <div class="inputs">
      <add-todo @newValue="addNewTodo" @badValue="emitBadValue" />
      <search-todo @search="searchTodo" />
    </div>
    <div v-if="todos.length > 0">
      <draggable
        :list="todos"
        :disabled="!enabled"
        item-key="name"
        class="todo-list"
        ghost-class="ghost"
        @start="dragging = true"
        @end="dragging = false"
      >
        <template #item="{ element }">
          <li
            @click="checkTodo(element.id)"
            class="todo-list-item"
            :class="{ 'not-draggable': !enabled, completed: element.completed }"
          >
            <div class="todo-checker"></div>
            <p>{{ element.text }}</p>
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
        <p>{{ todosLeft }} item(s) left</p>
        <div class="filters">
          <button class="filter-active" @click="filterAll($event)">All</button>
          <button @click="filterCompleted($event)">Completed</button>
        </div>
        <button class="clear" @click="clearCompleted">Clear Completed</button>
      </div>
    </div>
    <p v-else class="todo-list-item list-empty no-pointer">No todo's!</p>

    <!-- <ul v-if="todos.length > 0" class="todo-list">
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
    </ul>-->
  </div>
</template>

<script lang="ts">
import { defineComponent, onMounted, ref } from 'vue'
import draggable from 'vuedraggable'
import TodoItem from './TodoItem.vue'
import AddTodo from './AddTodo.vue'
import SearchTodo from './SearchTodo.vue'

export default defineComponent({
  name: 'TodoList',

  components: { TodoItem, AddTodo, SearchTodo, draggable },

  data() {
    return {
      enabled: true,
      dragging: false
    }
  },

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

    const todosLeft = ref(0)

    const addNewTodo = (newTodo: string) => {
      if (newTodo) {
        todos.value.push({ text: newTodo, completed: false, id: Math.random() })
        setTodosLeft(todos.value)
        newTodo = ''
      }
    }

    const deleteTodo = (id: number) => {
      todos.value = todos.value.filter((todo) => todo.id !== id)
      setTodosLeft(todos.value)
    }

    const emitBadValue = () => {
      emit('invalidValue')
    }

    const checkTodo = (id: number) => {
      let todo = todos.value.find((todo) => todo.id === id)
      if (todo) todo.completed = !todo.completed
      setTodosLeft(todos.value)
    }

    const clearCompleted = () => {
      todos.value = todos.value.filter((todo) => !todo.completed)
      setTodosLeft(todos.value)
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

    const setTodosLeft = (
      todos: {
        text: string
        completed: boolean
        id: number
      }[]
    ) => {
      localStorage.setItem('todos', JSON.stringify(todos))
      updateTodosCopy()
      let x = todos.filter((todo) => !todo.completed)
      todosLeft.value = x.length
    }

    onMounted(() => {
      let todosFromStorage
      setTodosLeft(todos.value)
      if (localStorage.getItem('todos')) {
        todosFromStorage = JSON.parse(localStorage.getItem('todos')!)
        todos.value = todosFromStorage
      }
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
      todosLeft
    }
  }
})
</script>
