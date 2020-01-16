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
                    @click="toggleTodo(assignment)">
            </label>
            <p>{{assignment.text}}</p>
            <div class="todo__remove--button"></div>
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
    isNewTodoEmpty () {
      return this.newTodo.trim() === '';
    },

    isNewTodoAlreadyExists () {
      const ids = this.createdTodos.map(todo => todo.text);

      return ids.indexOf(this.newTodo.trim()) > -1;
    },

    totalTodos () {
      return this.createdTodos.length;
    },

    lastId () {
      return (this.createdTodos[this.totalTodos - 1] || {}).id || 0;
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
    toggleTodo (triggeredTodo) {
      this.createdTodos.filter(todo => todo.id === triggeredTodo.id)[0].checked = !triggeredTodo.checked;

      this.$forceUpdate();
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
            }

            input {
                display: none;
            }

            .todo__remove--button {
                @include todo__remove--button(32px, $background__color--primary--light);
            }
        }
    }
</style>
