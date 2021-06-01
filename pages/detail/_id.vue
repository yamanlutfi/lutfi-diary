<template>
  <div class="container">
    {{this.$route.params.id}}test
   </div>
</template>

<script>
  export default {
    data() {
      return {
        mountains: []
      }
    },
    layout: 'frontend',
  }
</script>