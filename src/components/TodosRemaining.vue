<template>
    <div>{{remaining}} items left</div>
</template>

<script>
    export default {
        name: 'remaining-todos',
        props: {
          remaining: {
            type: Number,
            required: true
          }
        }
    }
</script>
