<template>
  <div class="container pt-5 mt-5">
    <div class="row">
      <div class="col-12 d-flex justify-content-center text-center">
        <h2>{{ form_data.options.goodbye_message.title }}</h2>
      </div>
      <div class="col-12 d-flex justify-content-center mt-3">
        <p>{{ form_data.options.goodbye_message.description }}</p>
      </div>
      <div class="col-12 d-flex justify-content-center mb-5 mt-3">
        <button @click.prevent="submitOnGoodbye" type="submit" class="btn btn-outline-primary">Submit</button>
      </div>
      <div class="col-12 text-center pt-5 text-info font-weight-bold" v-if="loadStatus === 'LOADING'">
        SUBMITTING...
      </div>
    </div>
  </div>
</template>

<script>
import formData from '../../form_data.json'

export default {
  name: "GoodbyeMessage",
  data() {
    return {
      form_data: formData,
      loadStatus: null
    }
  },
  methods: {
    submitOnGoodbye() {
      this.loadStatus = 'LOADING'
      setTimeout(() => {
        this.$emit('submitOnGoodbye')
      }, 600)
    }
  }
}
</script>

<style scoped>

</style>
