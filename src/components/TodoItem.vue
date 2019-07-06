<template>
  <div class="todo-item">
    <div class="todo-item-left">
      <input type="checkbox" v-model="completed"
             @change="doneEdit">
      <div v-if="!editing" @click="editTodo" class="todo-item-label"
           :class="{ 'completed-todo' : completed}">{{title}}
      </div>
      <input v-else class="todo-item-edit" type="text" v-model="title"
             @keyup.enter="doneEdit" @keyup.esc="cancelEdit" @blur="doneEdit" v-focus>
    </div>
    <div class="remove-item" @click="removeTodo(index)">
      &times;
    </div>
  </div>
</template>

<script>
  export default {
    name: 'todo-item',
    props: {
      todo: {
        type: Object,
        required: true,
      },
      index: {
        type: Number,
        required: true,
      },
      checkAll: {
        type: Boolean,
        required: true
      }
    },
    data() {
      return {
        'id': this.todo.id,
        'title': this.todo.title,
        'completed': this.todo.completed,
        'editing': this.todo.editing,
        'titleBeforeEdit': ''
      }
    },
    directives: {
      focus: {
        inserted: function (el) {
          el.focus()
        }
      }
    },
    watch: {
      checkAll () {
        this.completed = this.checkAll ? true : this.todo.completed
      }
    },
    methods: {
      removeTodo(index) {
        this.$emit('removedTodo', index)
      },
      editTodo() {
        this.titleBeforeEdit = this.title;
        this.editing = true
      },
      doneEdit() {
        if (this.title === '') {
          this.title = this.titleBeforeEdit
        }

        this.editing = false

        this.$emit('finishedEdit', {
          'index': this.index,
          'todo': {
            'id': this.id,
            'title': this.title,
            'completed': this.completed,
            'editing': this.editing
          }
        })
      },
      cancelEdit() {
        this.editing = false

        this.title = this.titleBeforeEdit
      }
    }
  }
</script>
