<template>
  <div class="col-12 mt-3 ml-4 d-inline-flex">
    <button type="submit" @submit.prevent="next" class="btn btn-dark ok-button">Ok</button>
    <p class="mt-2 ml-2"><span class="text-muted">Press </span>Enter</p>
  </div>
</template>

<script>
export default {
  name: "FormControlButtons",
  methods: {
    next() {
      this.$emit('next');
    }
  }
}
</script>

<style scoped>

</style>