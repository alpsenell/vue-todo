<template>
  <div id="todo">
    <input type="text" class="todo__input todo__input--large">
  </div>
</template>

<script>
export default {
  name: 'Todo',
};
</script>

<style scoped lang="scss">
  @import '../assets/sass/_variables';

  #todo {
    max-width: 50vw;

    input {
      border-bottom: 1px solid $color--input;
    }
  }
</style>
