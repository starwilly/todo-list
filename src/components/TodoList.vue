<template>
  <div class="hello">
    <div class="filters">
      <button @click="filterKey='all'">All</button>
      <button @click="filterKey='active'">Active</button>
      <button @click="filterKey='done'">Done</button>
    </div>
    <p>
    New Todo: <input type="text" v-model="newTodo" @keyup.enter="addTodo">
    </p>
    <ul class="todolist__content" v-for="todo in filteredTodos" :key="todo.id">
      <li class="todolist__item" :class="{ 'done': todo.done }">
        <input type="checkbox" v-model="todo.done">
        {{ todo.title }}
        <button>Edit</button>
        <button @click="removeTodo(dodo)">Delete</button>
      </li>
    </ul>
  </div>
</template>

<script>
const filters = {
  all: todos => todos,
  done: todos => todos.filter(t => t.done),
  active: todos => todos.filter(t => !t.done)
}

export default {
  name: 'TodoList',
  data () {
    return {
      filterKey: 'all',
      newTodo: '',
      todos: [
        {id: 1, title: 'test todo', done: true}
      ]
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
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}

.todolist__item.done {
  text-decoration: line-through;
}
</style>
