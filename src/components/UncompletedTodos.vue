<template>
    <div class="todo__un--completed--todos"
         :class="{ uncompleted: uncompletedTodos > 0 }">
        <span>{{ uncompletedTodos }}</span>
        <p> todos left</p>
    </div>
</template>

<script>
export default {
  name: 'UncompletedTodos',

  props: {
    uncompletedTodos: {
      required: true,
      type: Number,
      default: 0,
    },
  },
};
</script>

<style scoped lang="scss">
    @import '../assets/sass/_variables';

    .todo__un--completed--todos {
        color: $color__white;
        display: flex;
        align-items: center;
        color: $color__input--checked;

        &.uncompleted {
            color: red;
        }

        span {
            margin-right: 10px;
            font-size: 2em;
        }
    }
</style>
