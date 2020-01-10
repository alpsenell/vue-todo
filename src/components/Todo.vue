<template>
    <div id="todo">
        <input
                @keyup.enter="addNewTodo"
                @keyup.esc="cancelNewTodo"
                class="todo__input large"
                placeholder="What needs to be done?"
                type="text"
                v-model="newTodo">
        <div
                :key="assignment"
                class="todo__remaining__todos large"
                v-for="assignment in createdTodos">
        {{assignment}}
        </div>
    </div>
</template>

<script>
export default {
    name: 'Todo',

    data() {
        return {
            newTodo: '',
            createdTodos: [],
        };
    },

    methods: {
        addNewTodo() {
            this.createdTodos.push(this.newTodo.trim());
        },

        cancelNewTodo() {
            this.newTodo = '';
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
            color: $background__color--primary--light;
            width: 100%;
        }
    }
</style>
