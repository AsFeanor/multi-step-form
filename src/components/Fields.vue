<template>
  <div class="container mt-5">
    <div class="row justify-content-center">
      <div class="col-6">
        <form id="myForm" @submit.prevent="next">
          <div class="form-group container" @keyup.left="prev" v-if="activeStep.form.field_type_id === 7">
            <div class="row">
              <div class="col-12">
                <label for="email">{{ activeStep.form.label }}</label>
                <input type="email" class="form-control" id="email" v-model="forms[activeStep.form.unique_id]">
                <small class="form-text text-muted"
                       v-if="activeStep.form.is_required && forms[activeStep.form.unique_id].length === 0">Email is
                  required...</small>
                <p class="typo__p" v-if="submitStatus === 'ERROR'">Please fill the form correctly.</p>
                <p class="typo__p" v-if="submitStatus === 'PENDING'">Sending...</p>
              </div>
              <div class="col-12 mt-2 text-right">
                <button v-if="step !== 1" @click.prevent="prev" class="btn btn-outline-primary">Previous</button>
                <button type="submit" @submit.prevent="next" class="btn btn-outline-primary">Next</button>
              </div>
            </div>
          </div>
          <div class="form-group container" @keyup.enter="next" @keyup.left="prev"
               v-if="activeStep.form.field_type_id === 3">
            <div class="row">
              <div class="col-12">
                <label for="yes-no">{{ activeStep.form.label }}</label>
                <select class="form-control" id="yes-no" v-model="forms[activeStep.form.unique_id]">
                  <option v-for="(option, key) in activeStep.form.form_field_options" :key="key">
                    {{ option.option_label }}
                  </option>
                </select>
                <!--                          <div v-if="activeStep.form.is_required">Email is required!</div>-->
                <p class="typo__p" v-if="submitStatus === 'ERROR'">Please select a option.</p>
                <p class="typo__p" v-if="submitStatus === 'PENDING'">Sending...</p>
              </div>
              <div class="col-12 mt-2 d-flex justify-content-between">
                <button @click.prevent="prev" class="btn btn-outline-primary">Previous</button>
                <button @submit.prevent="next" class="btn btn-outline-primary">Next</button>
              </div>
            </div>
          </div>
          <div class="form-group container" @keyup.enter="next" @keyup.left="prev"
               v-if="activeStep.form.field_type_id === 4 && activeStep.form.extra.length === 0">
            <div class="row">
              <div class="col-12">
                <div>{{ activeStep.form.label }}</div>
                <div class="form-check form-check-inline d-flex justify-content-center"
                     v-for="(option, key) in activeStep.form.form_field_options" :key="key">
                  <input type="checkbox" class="form-check-input" :id="option.form_field_option_id"
                         :value="option.option_value" v-model="forms[activeStep.form.unique_id]">
                  <label :for="option.form_field_option_id" class="form-check-label">{{ option.option_label }}</label>
                </div>
                <p class="typo__p" v-if="submitStatus === 'ERROR'">Please select at least one of the checkboxes</p>
                <p class="typo__p" v-if="submitStatus === 'PENDING'">Sending...</p>
              </div>
              <div class="col-12 mt-2 d-flex justify-content-between">
                <button @click.prevent="prev" class="btn btn-outline-primary">Previous</button>
                <button @submit.prevent="next" class="btn btn-outline-primary">Next</button>
              </div>
            </div>
          </div>
          <div class="form-group container" @keyup.enter="next" @keyup.left="prev"
               v-if="activeStep.form.field_type_id ===6">
            <div class="row">
              <div class="col-12 d-flex justify-content-center">
                <label for="time-start" class="placeholder">{{ activeStep.form.label }}</label>
              </div>
              <div class="col-12 d-flex justify-content-center">
                <input id="time-start" class="input" type="datetime-local" placeholder=" "
                       v-model="forms[activeStep.form.unique_id]"/>
              </div>
              <p class="typo__p" v-if="submitStatus === 'ERROR'">Please select a date.</p>
              <p class="typo__p" v-if="submitStatus === 'PENDING'">Sending...</p>
              <div class="col-12 mt-2 d-flex justify-content-between">
                <button @click.prevent="prev" class="btn btn-outline-primary">Previous</button>
                <button @submit.prevent="next" class="btn btn-outline-primary">Next</button>
              </div>
            </div>
          </div>
          <div class="form-group container" @keyup.enter="next" @keyup.left="prev"
               v-if="activeStep.form.field_type_id ===8">
            <div class="row">
              <div class="col-12">
                <label for="phone">{{ activeStep.form.label }}</label>
                <input type="tel" class="form-control" id="phone" v-model="forms[activeStep.form.unique_id]">
                <p class="typo__p" v-if="submitStatus === 'ERROR'">Please fill the form correctly.</p>
                <p class="typo__p" v-if="submitStatus === 'PENDING'">Sending...</p>
              </div>
              <div class="col-12 mt-2 d-flex justify-content-between">
                <button @click.prevent="prev" class="btn btn-outline-primary">Previous</button>
                <button @submit.prevent="next" class="btn btn-outline-primary">Next</button>
              </div>
            </div>
          </div>
          <div class="form-group container" @keyup.enter="next" @keyup.left="prev"
               v-if="activeStep.form.field_type_id === 23">
            <div class="row">
              <div class="col-12">
                <label for="name">{{ activeStep.form.label }}</label>
                <input type="text" class="form-control" id="name" v-model="forms[activeStep.form.unique_id]"><br>
                <p class="typo__p" v-if="submitStatus === 'ERROR'">Please fill the form correctly.</p>
                <p class="typo__p" v-if="submitStatus === 'PENDING'">Sending...</p>
              </div>
              <div class="col-12 mt-2 d-flex justify-content-between">
                <button @click.prevent="prev" class="btn btn-outline-primary">Previous</button>
                <button @submit.prevent="next" type="submit" :disabled="submitStatus === 'PENDING' || !isValid"
                        class="btn btn-outline-primary">Next
                </button>
              </div>
            </div>
          </div>
          <div class="form-group container" @keyup.enter="next" @keyup.left="prev"
               v-if="activeStep.form.field_type_id ===24">
            <div class="row">
              <div class="col-12">
                <label for="surname">{{ activeStep.form.label }}</label>
                <input type="text" class="form-control" id="surname" v-model="forms[activeStep.form.unique_id]">
                <p class="typo__p" v-if="submitStatus === 'ERROR'">Please fill the form correctly.</p>
                <p class="typo__p" v-if="submitStatus === 'PENDING'">Sending...</p>
              </div>
              <div class="col-12 mt-2 d-flex justify-content-between">
                <button @click.prevent="prev" class="btn btn-outline-primary">Previous</button>
                <button @submit.prevent="next" class="btn btn-outline-primary">Next</button>
              </div>
            </div>
          </div>
          <div class="form-group container" @keyup.enter.prevent="submitForm" @keyup.left="prev"
               v-if="activeStep.form.field_type_id === 4 && activeStep.form.extra.field_type === 'legacy'">
            <div class="row">
              <div class="col-12">
                <div class="form-check form-check-inline">
                  <label for="last" class="form-check-label">Please confirm that it is okay for us to contact you about
                    this information as well as products and services.
                    (You will always be given the right to unsubscribe at any point in the future)</label>
                  <input type="checkbox" id="last" class="form-check-input" :value="activeStep.form.form_field_options[0].option_value" v-model="forms[activeStep.form.unique_id]">
                  <p class="typo__p" v-if="submitStatus === 'ERROR'">You have to confirm that.</p>
                  <p class="typo__p" v-if="submitStatus === 'PENDING'">Sending...</p>
                </div>
              </div>
              <div class="col-12 mt-2 d-flex justify-content-between">
                <button @click.prevent="prev" class="btn btn-outline-primary">Previous</button>
                <button @click.prevent="submitForm" class="btn btn-outline-primary" type="submit">Submit form</button>
              </div>
            </div>
          </div>
        </form>
      </div>
      <div class="col-6">
        <pre>{{ errors }}</pre>
        <pre>{{ forms }}</pre>
      </div>
    </div>
  </div>
</template>

<script>
import formData from '../../form_data.json';
import {required, minLength, email} from 'vuelidate/lib/validators'


export default {
  name: "Fields",
  data() {
    return {
      forms: {},
      errors: [],
      step: 1,
      form_data: formData,
      activeStep: {stepId: 0, form: formData[0]},
      submitStatus: null
    }
  },
  mounted() {
    const formsData = this.forms
    $.each(formData, (key, value) => {
      this.$set(formsData, value.unique_id, [])
    })
    console.log(formsData)
    return formsData

  },
  methods: {
    prev() {
      this.activeStep.stepId--;
      this.activeStep.form = this.form_data[this.activeStep.stepId];
    },
    next() {
      if (this.isValid()) {
        if (this.validEmail(this.email)) {
          this.errors.push('Valid email required.');
        } else {
          this.submitStatus = 'PENDING'
          setTimeout(() => {
            this.submitStatus = 'OK'
            this.activeStep.stepId++;
            this.activeStep.form = this.form_data[this.activeStep.stepId];
            this.errors.pop()
          }, 600)
        }
      } else {
        this.errors.push("This is required")
        this.submitStatus = 'ERROR'
        setTimeout(() => {
          this.errors.pop()
        }, 2000)
        console.log("Invalid")
      }
    },
    validEmail(email) {
      const re = /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
      return re.test(email);
    },
    isValid() {
      return !this.activeStep.form.is_required || (this.forms[this.activeStep.form.unique_id] && this.forms[this.activeStep.form.unique_id].length);
    },
    submitForm() {
      console.log(this.forms)
    }
  },
  computed: {
    // formsData() {
    //   const formsData = this.forms
    //   $.each(formData, (key, value) => {
    //     formsData.push(value.unique_id)
    //   })
    // }
  },
}
</script>

<style scoped>

</style>
