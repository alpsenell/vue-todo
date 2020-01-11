<template>
  <div id="app">
    <todo></todo>
  </div>
</template>

<script>
import Todo from './components/Todo.vue';

export default {
    name: 'app',
    components: {
        Todo,
    },
};
</script>

<style lang="scss">
  @import './assets/sass/_variables';
  @import './assets/sass/_mixins';

  body, html {
    @include reset;
    @include center;
    @include viewport__view;
    font-family: $font__family--primary;
    background-color: $background__color--primary--dark;
    color: $color--primary;

    *:focus {
      outline: none;
    }

    #app {
      @include app__view;
      @include center;
      height: 100%;
    }
  }
</style>
