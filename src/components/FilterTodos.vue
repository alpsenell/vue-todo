<template>
  <div class="filtering-wrapper">
    <div class="filters">
      <button :class="{ active : filter === 'all' }" @click="changeFilter('all')">All</button>
      <button :class="{ active : filter === 'active' }" @click="changeFilter('active')">Active</button>
      <button :class="{ active : filter === 'completed' }" @click="changeFilter('completed')">Completed</button>
    </div>
    <div>
      <button v-if="showClearCompletedButton" @click="clearCompleted">Clear Completed</button>
    </div>
  </div>
</template>

<script>
    export default {
      name: 'todo-filter',
      data() {
        return {
          'filter': 'all'
        }
      },
      methods: {
        changeFilter(filter) {
          eventBus.$emit('filterChanged', filter)
        }
      }
    }
</script>

<style scoped>

</style>
