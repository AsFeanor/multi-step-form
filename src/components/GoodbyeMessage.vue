<template>
  <div class="container pt-5 mt-5">
    <div class="row">
      <div class="col-12 d-flex justify-content-center text-center"
           v-if="form_data.options.goodbye_message.title != null">
        <h2>{{ form_data.options.goodbye_message.title }}</h2>
      </div>
      <div class="col-12 d-flex justify-content-center mt-3" v-if="form_data.options.goodbye_message">
        <p>{{ form_data.options.goodbye_message.description }}</p>
      </div>
      <div class="col-12 d-flex justify-content-center mb-5 mt-3 text-center" v-if="form_data.success_message != null">
        {{ form_data.success_message }}
      </div>
      <div class="col-12 d-flex justify-content-center mb-5 mt-3 text-center" v-if="form_data.redirect_url != null">
        You can safely close this page or you will be redirected to <a href=""> {{ form_data.redirect_url }} </a> in
        {{ countDown }} seconds.
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
      countDown: 5
    }
  },
  created() {
    this.redirect()
  },
  methods: {
    redirect() {
      setTimeout(() => {
        if (this.countDown > 0) {
          this.countDown -= 1
          this.redirect()
        } else {
          window.open(this.form_data.redirect_url, '_self')
        }
      }, 1000)
    }
  }
}
</script>

<style scoped>

</style>
