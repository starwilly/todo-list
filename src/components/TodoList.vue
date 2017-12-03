<template>
  <div class="hello offset-md-2 col-md-8 mt-5">
    <div class="row">
      <div class="btn-group filters mx-auto">
        <button class="btn btn-light"
                :class="{active: filterKey==='all'}"
                @click="filterKey='all'">All</button>
        <button class="btn btn-light"
                :class="{active: filterKey==='active'}"
                @click="filterKey='active'">Active</button>
        <button class="btn btn-light"
                :class="{active: filterKey==='done'}"
                @click="filterKey='done'">Done</button>
      </div>
    </div>
    <div class="row justify-content-end toolbar">
      <button class="btn btn-link"
        @click="clearDoneTodos"
        v-show="filterKey ==='done'">Clear completed</button>
    </div>

    <p class="row">
      <div class="input-group">
        <input type="text" class="form-control input--new-todo"
          placeholder="What needs to be done?"
          v-model="newTodo"
          @keyup.enter="addTodo">
        <span class="input-group-btn">
          <button class="btn btn-primary" type="button" @click="addTodo">
            <i class="zmdi zmdi-plus"></i> New Todo
          </button>
        </span>
      </div>
    </p>

    <ul class="list-group todolist__content">
      <todo-list-item
        v-for="todo in filteredTodos"
        :key="todo.id"
        :todo="todo"
        :editTodo="editTodo"
        :done-editing-todo="doneEditingTodo"
        :remove-todo="removeTodo"
        :cancel-editing-todo="cancelEditingTodo"
        :is-editing="todo === editingTodo" />
    </ul>

  </div>
</template>

<script>

import TodoListItem from '@/components/TodoListItem'

const filters = {
  all: todos => todos,
  done: todos => todos.filter(t => t.done),
  active: todos => todos.filter(t => !t.done)
}

export default {
  name: 'TodoList',
  components: {
    TodoListItem
  },
  data () {
    return {
      filterKey: 'all',
      newTodo: '',
      todos: [],
      editingTodo: null,
      titleBeforeEditing: ''
    }
  },
  computed: {
    filteredTodos () {
      return filters[this.filterKey](this.todos)
    }
  },
  methods: {
    addTodo () {
      const title = this.newTodo && this.newTodo.trim()
      if (!title) {
        return
      }
      this.todos.push({
        id: Date.now(),
        title,
        done: false
      })
      this.newTodo = ''
    },
    removeTodo (todo) {
      this.todos.splice(this.todos.indexOf(todo), 1)
    },
    editTodo (todo) {
      this.titleBeforeEditing = todo.title
      this.editingTodo = todo
    },
    cancelEditingTodo (todo) {
      this.editingTodo = null
      todo.title = this.titleBeforeEditing
    },
    doneEditingTodo (todo) {
      this.editingTodo = null
    },
    clearDoneTodos () {
      this.todos = filters['active'](this.todos)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.input--new-todo::placeholder {
  color: #cfcfcfcf;
}

.toolbar {
  height: 40px;
}

</style>
