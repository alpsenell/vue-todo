<template>
    <div>
        <div><input type="checkbox" :checked="!anyRemaining" @change="checkAllTodos">Check All</div>
        <input type="text" class="todo-input" placeholder="What needs to be done" v-model="newTodo" @keyup.enter="addTodo">
      <transition-group name="focus-in">
        <div v-for="(todo,index) in todosFiltered" :key="todo.id" class="todo-item">
          <div class="todo-item-left">
            <input type="checkbox" v-model="todo.completed">
            <div v-if="!todo.editing" @click="editTodo(todo)" class="todo-item-label"
                 :class="{ 'completed-todo' : todo.completed}">{{todo.title}}
            </div>
            <input v-else class="todo-item-edit" type="text" v-model="todo.title"
                   @keyup.enter="doneEdit(todo)" @keyup.esc="cancelEdit(todo)" @blur="doneEdit(todo)" v-focus>
          </div>
          <div class="remove-item" @click="removeTodo(index)">
            &times;
          </div>
        </div>
      </transition-group>
        <div> {{ remainingTodos }} Todos left!</div>
        <div class="filtering-wrapper">
            <div>
                <button :class="{ active : filter === 'all' }" @click="filter ='all'">All</button>
                <button :class="{ active : filter === 'active' }" @click="filter = 'active'">Active</button>
                <button :class="{ active : filter === 'completed' }" @click="filter = 'completed'">Completed</button>
            </div>
            <div>
                <button v-if="showClearCompletedButton" @click="clearCompleted">Clear Completed</button>
            </div>
        </div>
    </div>
</template>

<script>
export default {
  name: 'todo-list',
  data () {
    return {
      newTodo: '',
      idForTodo: 3,
      titleBeforeEdit: '',
      filter: 'all',
      todos: [
          {
              'id': 1,
              'title': 'Finish Vue Screencast',
              'completed': false,
              'editing': false
          },
          {
              'id': 2,
              'title': 'Take over patates',
              'completed': false,
              'editing': false
          }
      ]
    }
  },
  computed: {
      remainingTodos () {
          return this.todos.filter(todo => !todo.completed).length
      },
      anyRemaining () {
          return this.remainingTodos != 0
      },
      todosFiltered () {
          if (this.filter === 'all') {
              return this.todos
          } else if (this.filter === 'active') {
              return this.todos.filter(todo => !todo.completed)
          } else if (this.filter === 'completed') {
              return this.todos.filter(todo => todo.completed)
          }

          return this.todos
      },
      showClearCompletedButton () {
          return this.todos.filter(todo => todo.completed).length > 0
      }
  },
  directives: {
      focus: {
          inserted: function (el) {
              el.focus()
          }
      }
  },
  methods: {
      addTodo () {
          if (this.newTodo.trim() === '') {
            return;
          }

        this.todos.push({
          id: this.idForTodo,
          title: this.newTodo,
          completed: false,
          editing: false
        });

        this.newTodo = '';
        this.idForTodo++;
      },
      removeTodo(index) {
          this.todos.splice(index, 1);
      },
      editTodo (todo) {
          this.titleBeforeEdit = todo.title

          todo.editing = true
      },
      cancelEdit (todo) {
          todo.editing = false

          todo.title = this.titleBeforeEdit
      },
      doneEdit (todo) {
          if (todo.title === '') {
              todo.title = this.titleBeforeEdit
          }

          todo.editing = false
      },
      checkAllTodos () {
          this.todos.forEach((todo) => todo.completed = event.target.checked)
      },
      clearCompleted () {
          this.todos = this.todos.filter(todo => !todo.completed)
      }
  }
}
</script>

<style scoped lang="scss">
    .todo-input {
      width: 100%;
      padding: 10px 18px;
      font-size: 18px;
      margin-bottom: 16px;

      &:focus {
        outline: 0;
      }
    }

    .todo-item {
      display: flex;
      align-items: center;
      justify-content: space-between;
    }

    .todo-item-left {
      display: flex;
      align-items: center;
    }

    .todo-item-label {
      padding: 10px;
      border: 1px solid white;
      margin-left: 12px;
    }

    .todo-item-edit {
      font-size: 18px;
      color: #2c3e50;
      margin-left: 12px;
      width: 100%;
      padding: 10px;
      border: 1px solid #ccc;
      font-family: 'Avenir', Helvetica, Arial, sans-serif;

      &:focus {
        outline: none;
      }
    }

    .remove-item {
      cursor: pointer;

      &:hover {
        color: black;
      }
    }

    .completed-todo {
      text-decoration: line-through;
      color: gray;
    }

    .complete-all-todos button {
      padding: 5px 15px;
      background-color: rgb(215, 202, 202);
      cursor: pointer;
      border: none;
      box-shadow: none;
    }

    .active {
        background-color: goldenrod;

        &:focus {
            outline: none;
        }
    }

    .focus-in-enter-active {
      animation: focus-in-contract-bck .5s cubic-bezier(0.215, 0.610, 0.355, 1.000) both;
    }

    .focus-in-leave-active {
      animation: blur-out-expand-fwd .5s cubic-bezier(0.250, 0.460, 0.450, 0.940) both;
    }

    @-webkit-keyframes focus-in-contract-bck {
      0% {
        letter-spacing: 1em;
        -webkit-transform: translateZ(300px);
        transform: translateZ(300px);
        -webkit-filter: blur(12px);
        filter: blur(12px);
        opacity: 0;
      }
      100% {
        -webkit-transform: translateZ(12px);
        transform: translateZ(12px);
        -webkit-filter: blur(0);
        filter: blur(0);
        opacity: 1;
      }
    }
    @keyframes focus-in-contract-bck {
      0% {
        letter-spacing: 1em;
        -webkit-transform: translateZ(300px);
        transform: translateZ(300px);
        -webkit-filter: blur(12px);
        filter: blur(12px);
        opacity: 0;
      }
      100% {
        -webkit-transform: translateZ(12px);
        transform: translateZ(12px);
        -webkit-filter: blur(0);
        filter: blur(0);
        opacity: 1;
      }
    }

    @-webkit-keyframes blur-out-expand-fwd {
      0% {
        -webkit-transform: translateZ(0);
        transform: translateZ(0);
        -webkit-filter: blur(0.01);
        filter: blur(0.01);
      }
      100% {
        letter-spacing: 1em;
        -webkit-transform: translateZ(300px);
        transform: translateZ(300px);
        -webkit-filter: blur(12px) opacity(0%);
        filter: blur(12px) opacity(0%);
      }
    }
    @keyframes blur-out-expand-fwd {
      0% {
        -webkit-transform: translateZ(0);
        transform: translateZ(0);
        -webkit-filter: blur(0.01);
        filter: blur(0.01);
      }
      100% {
        letter-spacing: 1em;
        -webkit-transform: translateZ(300px);
        transform: translateZ(300px);
        -webkit-filter: blur(12px) opacity(0%);
        filter: blur(12px) opacity(0%);
      }
    }
</style>
