<template>
  <li class="todolist__item" :class="{ 'done': todo.done }">
    <input type="checkbox" v-model="todo.done">
    <label @dblclick="editTodo(todo)" v-show="!isEditing">
      {{ todo.title }}
    </label>
    <input type="text"
            v-show="isEditing"
            v-todo-focus="isEditing"
            v-model="todo.title"
            @blur="doneEditingTodo(todo)"
            @keyup.enter="doneEditingTodo(todo)"
            @keyup.esc="cancelEditingTodo(todo)" />
    <button @click="removeTodo(todo)">Delete</button>
  </li>
</template>

<<script>
export default {
  name: 'TodoListItem',
  props: {
    todo: Object,
    isEditing: {
      type: Boolean,
      default: false
    },
    editTodo: {
      type: Function,
      required: true
    },
    removeTodo: {
      type: Function,
      required: true
    },
    doneEditingTodo: {
      type: Function,
      required: true
    },
    cancelEditingTodo: {
      type: Function,
      required: true
    }
  },
  directives: {
    todoFocus: (el, binding) => {
      if (binding.value) {
        el.focus()
      }
    }
  }
}
</script>

