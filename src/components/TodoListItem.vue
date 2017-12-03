<template>
  <li class="list-group-item todolist__item d-flex align-items-stretch" :class="{ 'done': todo.done }">
    <div class="checkbox align-self-center">
      <div class="pretty p-icon p-round p-smooth ">
          <input type="checkbox" v-model="todo.done">
          <div class="state p-primary">
              <i class="icon zmdi zmdi-check"></i>
              <label></label>
          </div>
      </div>

    </div>
    <label class="title" @dblclick="editTodo(todo)" v-show="!isEditing">
      {{ todo.title }}
    </label>
    <input type="text"
      class="input--edit"
      v-show="isEditing"
      v-todo-focus="isEditing"
      v-model="todo.title"
      @blur="doneEditingTodo(todo)"
      @keyup.enter="doneEditingTodo(todo)"
      @keyup.esc="cancelEditingTodo(todo)" />
    <button class="delete-btn text-danger" @click="removeTodo(todo)">
      <i class="zmdi zmdi-close"></i>
    </button>

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

<style lang="scss" scoped>

.todolist__item {
  .title {
    display: block;
    padding: 0;
    margin: 0;
    width: 100%;
    word-break: break-all;
  }

  &.done .title {
    color: #ccc;
    text-decoration: line-through;
  }
}

.delete-btn {
  display: none;
  background: none;
  border: 0;
  padding: 0 5px;
  cursor: pointer;
  width: 40px;
}

.todolist__item:hover .delete-btn {
  display: block;
}

.input--edit {
  width: 100%;
  border: 0;
  &:focus {
    outline: none;
  }
}

</style>
