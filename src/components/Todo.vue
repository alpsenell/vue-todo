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
                v-for="(assignment, index) in createdTodos">
            <input
                    :id="'todo__checkbox-' + index"
                    type="checkbox">
            <label
                    class="todo__checkbox"
                    :for="'todo__checkbox-' + index"></label>
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
    @import '../assets/sass/_keyFrames';

    $checkbox--height: 25px;
    $checkbox__animation--duration: 0.4s;

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
            text-align: center;

            input {
                display: none;
            }

            .todo__checkbox {
                @include todo__checkbox($checkbox--height, $checkbox__animation--duration);
            }

            input:checked + .todo__checkbox,
            .todo__checkbox.checked{
                @include todo__checkbox--keyFrames($checkbox--height);
                border-color: $color__input--checked;

                &::after{
                    height: $checkbox--height * .5;
                    @include animation('inputBottomCheck 0.2s ease 0s forwards');
                }

                &::before{
                    height: $checkbox--height * 1.2;
                    @include animation('inputTopCheck 0.4s ease 0s forwards');
                }
            }
        }
    }
</style>
