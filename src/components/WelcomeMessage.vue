<template>
  <div class="container pt-5 mt-5">
    <div class="row">
      <div class="col-12 d-flex justify-content-center text-center">
        <h2>{{ form_data.options.welcome_message.title }}</h2>
      </div>
      <div class="col-12 d-flex justify-content-center mt-3">
        <p>{{ form_data.options.welcome_message.description }}</p>
      </div>
      <div class="col-12 d-flex justify-content-center mb-5 mt-3">
        <button @click.prevent="goToForm" type="submit" class="btn btn-outline-primary">Go to form</button>
      </div>
      <div class="col-12 text-center pt-5 text-info font-weight-bold" v-if="loadStatus === 'LOADING'">
        LOADING...
      </div>
    </div>
  </div>
</template>

<script>
import formData from '../../form_data.json'

export default {
  name: "WelcomeMessage",
  data() {
    return {
      form_data: formData,
      loadStatus: null
    }
  },
  methods: {
    goToForm() {
      this.loadStatus = 'LOADING'
      setTimeout(() => {
        this.$emit('goToForm')
      }, 600)
    }
  }
}
</script>

<style scoped>

</style>
