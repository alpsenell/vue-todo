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
    @include clearfix;
  }

  #app {
    font-family: $font__family--primary;
    background-color: $background__color--primary--dark;
    color: $color--primary;
    @include center;
    @include viewport__view;
  }
</style>
