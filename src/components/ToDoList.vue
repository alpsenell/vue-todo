<template>
  <div>
    <div><input type="checkbox" :checked="!anyRemaining" @change="checkAllTodos">Check All</div>
    <input type="text" class="todo-input" placeholder="What needs to be done" v-model="newTodo" @keyup.enter="addTodo">
    <transition-group name="focus-in">
      <todo-item v-for="(todo,index) in todosFiltered"
                 key="todo.id" :todo="todo" :index="index" :checkAll="!anyRemaining"
                 @removedTodo="removeTodo" @finishedEdit="finishedEdit">
      </todo-item>
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
  import TodoItem from './TodoItem'

  export default {
    name: 'todo-list',
    components: {TodoItem},
    comments: {
      TodoItem,
    },
    data() {
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
      remainingTodos() {
        return this.todos.filter(todo => !todo.completed).length
      },
      anyRemaining() {
        return this.remainingTodos !== 0
      },
      todosFiltered() {
        if (this.filter === 'all') {
          return this.todos
        } else if (this.filter === 'active') {
          return this.todos.filter(todo => !todo.completed)
        } else if (this.filter === 'completed') {
          return this.todos.filter(todo => todo.completed)
        }

        return this.todos
      },
      showClearCompletedButton() {
        return this.todos.filter(todo => todo.completed).length > 0
      }
    },
    methods: {
      addTodo() {
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
      checkAllTodos() {
        this.todos.forEach((todo) => todo.completed = event.target.checked)
      },
      clearCompleted() {
        this.todos = this.todos.filter(todo => !todo.completed)
      },
      finishedEdit (data) {
        this.todos.splice(data.index, 1, data.todo)
      }
    }
  }
</script>

<style lang="scss">
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
