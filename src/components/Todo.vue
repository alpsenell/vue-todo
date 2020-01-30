<template>
    <div id="todo">
        <input
                @keyup.enter="addNewTodo"
                @keyup.esc="resetTodo"
                class="todo__input large"
                placeholder="What needs to be done?"
                type="text"
                v-model="newTodo">
        <div
                :key="assignment.id"
                class="todo__remaining__todos large"
                v-for="(assignment, index) in createdTodos">
            <input
                    :id="'todo__checkbox-' + index"
                    type="checkbox">
            <label
                    class="todo__checkbox"
                    :for="'todo__checkbox-' + index"
                    @click="toggleTodo(assignment, $event)">
            </label>
            <p>{{assignment.text}}</p>
            <div
                    class="todo__remove--button"
                    @click="removeTodo(assignment)">
            </div>
        </div>
        <div class="todo__total__todos">
            <p>{{ unCompletedTodos }} todos left</p>
        </div>
    </div>
</template>

<script>
export default {
  name: 'Todo',

  data () {
    return {
      newTodo: '',
      createdTodos: [],
    };
  },

  computed: {
    /**
     * @returns {string}
     */
    isNewTodoEmpty () {
      return this.newTodo.trim() === '';
    },

    /**
     * @returns {boolean}
     */
    isNewTodoAlreadyExists () {
      const ids = this.createdTodos.map(todo => todo.text);

      return ids.indexOf(this.newTodo.trim()) > -1;
    },

    /**
     * @returns {number}
     */
    totalTodos () {
      return this.createdTodos.length;
    },

    /**
     * @returns {number}
     */
    lastId () {
      return (this.createdTodos[this.totalTodos - 1] || {}).id || 0;
    },

    unCompletedTodos () {
      return this.createdTodos.filter(todo => !todo.checked).length;
    },
  },

  methods: {
    addNewTodo () {
      if (this.isNewTodoEmpty || this.isNewTodoAlreadyExists) {
        return;
      }

      this.createdTodos.push(
        this.createTodoObject(
          this.newTodo.trim(),
        ),
      );

      this.resetTodo();
    },

    resetTodo () {
      this.newTodo = '';
    },

    /**
     * @param {string} todoName
     * @returns {object}
     */
    createTodoObject (todoName) {
      return {
        text: todoName,
        id: this.lastId + 1,
        checked: false,
      };
    },

    /**
     * @param {object} triggeredTodo
     * @param {boolean} triggeredTodo.checked
     */
    toggleTodo (triggeredTodo, event) {
      const todoText = event.target.nextSibling;
      this.createdTodos.filter(todo => todo.id === triggeredTodo.id)[0].checked = !triggeredTodo.checked;

      if (triggeredTodo.checked) {
        todoText.className += 'todo__checked--text';
      } else {
        todoText.className = todoText.className.replace('todo__checked--text', '');
      }

      this.$forceUpdate();
    },

    /**
     * @param {object} triggeredTodo
     * @param {number} triggeredTodo.id
     */
    removeTodo (triggeredTodo) {
      this.createdTodos.splice(this.createdTodos.findIndex(todos => todos.id === triggeredTodo.id), 1);
    },
  },

};
</script>

<style scoped lang="scss">
    @import '../assets/sass/_variables';
    @import '../assets/sass/_mixins';

    #todo {
        @include font__size--main;
        @include center;
        flex-direction: column;
        align-items: center;
        width: 100%;

        .large {
            @include large;
        }

        .todo__input {
            @include todo__input;

            &::placeholder {
                color: $background__color--primary--light;
            }
        }

        .todo__remaining__todos {
            $checkbox--height: 25px;
            $checkbox__animation--duration: 0.4s;

            @include todo__checkbox--unchecked($checkbox--height, $checkbox__animation--duration);
            @include todo__checkbox--checked($checkbox--height);
            color: $background__color--primary--light;
            display: flex;
            justify-content: space-between;
            margin: 10px;

            p {
                @include reset;

                &.todo__checked--text {
                    text-decoration: line-through;
                    color: $color__input--checked;
                }
            }

            input {
                display: none;
            }

            .todo__remove--button {
                @include todo__remove--button(32px, $background__color--primary--light);
            }
        }

        .todo__total__todos {
            color: $color__white;
        }
    }
</style>
