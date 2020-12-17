<template>
  <div class="container mt-5">
    <WelcomeMessage @goToForm="goToForm" v-if="welcomeExist === true"></WelcomeMessage>
    <div v-if="welcomeExist === false && goodbyeExist === false" class="row justify-content-center">
      <div class="col-12 mb-5">
        <div class="progress">
          <div class="progress-bar" role="progressbar"
               :style="{ width: ((activeStep.stepId + 1)/(formLength/100)) + '%' }"
               :aria-valuenow="activeStep.stepId + 1" aria-valuemin="0"
               :aria-valuemax="formLength">{{ activeStep.stepId + 1 }}
          </div>
        </div>
      </div>
      <div class="col-12">
        <form id="myForm" @submit.prevent="next">
          <div class="form-group container" @keyup.left="prev" v-if="activeStep.form.field_type_id === 7">
            <div class="row">
              <div class="col-12">
                <label for="email">{{ activeStep.form.label }}
                  <p v-if="activeStep.form.is_required" class="text-danger d-inline">*</p>
                </label>
                <input type="email" class="form-control" id="email" v-model="forms[activeStep.form.unique_id]">
                <small class="form-text text-muted"
                       v-if="activeStep.form.is_required && forms[activeStep.form.unique_id].length === 0">Email is
                  required...</small> <!-- TODO -->
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
                <label for="yes-no">{{ activeStep.form.label }}
                  <p v-if="activeStep.form.is_required" class="text-danger d-inline">*</p>
                </label>
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
                <div>{{ activeStep.form.label }}
                  <p v-if="activeStep.form.is_required" class="text-danger d-inline">*</p>
                </div>
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
                <label for="time-start" class="placeholder">{{ activeStep.form.label }}
                  <p v-if="activeStep.form.is_required" class="text-danger d-inline">*</p>
                </label>
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
               v-if="activeStep.form.field_type_id === 8">
            <div class="row">
              <div class="col-12">
                <label for="phone">{{ activeStep.form.label }}
                  <p v-if="activeStep.form.is_required" class="text-danger d-inline">*</p>
                </label>
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
                <label for="name">{{ activeStep.form.label }}
                  <p v-if="activeStep.form.is_required" class="text-danger d-inline">*</p>
                </label>
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
               v-if="activeStep.form.field_type_id === 24">
            <div class="row">
              <div class="col-12">
                <label for="surname">{{ activeStep.form.label }}
                  <p v-if="activeStep.form.is_required" class="text-danger d-inline">*</p>
                </label>
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
          <div class="form-group container" @keyup.enter="next" @keyup.left="prev"
               v-if="activeStep.form.field_type_id === 2">
            <div class="row">
              <div class="col-12">
                <label for="number">{{ activeStep.form.label }}
                  <p v-if="activeStep.form.is_required" class="text-danger d-inline">*</p>
                </label>
                <input type="number" class="form-control" id="number" v-model="forms[activeStep.form.unique_id]">
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
               v-if="activeStep.form.field_type_id === 5">
            <div class="row">
              <div class="col-12">
                <div>{{ activeStep.form.label }}
                  <p v-if="activeStep.form.is_required" class="text-danger d-inline">*</p>
                </div>
                <div class="form-check form-check-inline d-flex justify-content-center"
                     v-for="(option, key) in activeStep.form.form_field_options" :key="key">
                  <input type="radio" class="form-check-input" :id="option.form_field_option_id"
                         :value="option.option_value" v-model="forms[activeStep.form.unique_id]">
                  <label :for="option.form_field_option_id" class="form-check-label">{{ option.option_label }}</label>
                </div>
                <p class="typo__p" v-if="submitStatus === 'ERROR'">Please select one of the options.</p>
                <p class="typo__p" v-if="submitStatus === 'PENDING'">Sending...</p>
              </div>
              <div class="col-12 mt-2 d-flex justify-content-between">
                <button @click.prevent="prev" class="btn btn-outline-primary">Previous</button>
                <button @submit.prevent="next" class="btn btn-outline-primary">Next</button>
              </div>
            </div>
          </div>
          <div class="form-group container" @keyup.enter="next" @keyup.left="prev"
               v-if="activeStep.form.field_type_id === 9">
            <div class="row">
              <div class="col-12">
                <label for="textarea">{{ activeStep.form.label }}
                  <p v-if="activeStep.form.is_required" class="text-danger d-inline">*</p>
                </label>
                <textarea id="textarea" class="form-control" cols="30" rows="10"
                          v-model="forms[activeStep.form.unique_id]"></textarea>
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
               v-if="activeStep.form.field_type_id === 11">
            <div class="row">
              <div class="col-12">
                <div>{{ activeStep.form.label }}
                  <p v-if="activeStep.form.is_required" class="text-danger d-inline">*</p>
                </div>
                <div class="form-check form-check-inline d-flex justify-content-center"
                     v-for="(option, key) in activeStep.form.form_field_options" :key="key">
                  <input type="radio" class="form-check-input" :id="option.form_field_option_id"
                         :value="option.option_value" v-model="forms[activeStep.form.unique_id]">
                  <label :for="option.form_field_option_id" class="form-check-label">{{ option.option_label }}</label>
                </div>
                <p class="typo__p" v-if="submitStatus === 'ERROR'">Please choose one of the options.</p>
                <p class="typo__p" v-if="submitStatus === 'PENDING'">Sending...</p>
              </div>
              <div class="col-12 mt-2 d-flex justify-content-between">
                <button @click.prevent="prev" class="btn btn-outline-primary">Previous</button>
                <button @submit.prevent="next" class="btn btn-outline-primary">Next</button>
              </div>
            </div>
          </div>
          <div class="form-group container" @keyup.enter="next" @keyup.left="prev"
               v-if="activeStep.form.field_type_id === 26">
            <div class="row">
              <div class="col-12">
                <label for="salutation">{{ activeStep.form.label }}
                  <p v-if="activeStep.form.is_required" class="text-danger d-inline">*</p>
                </label>
                <select class="form-control" id="salutation" v-model="forms[activeStep.form.unique_id]">
                  <option v-for="(option, key) in activeStep.form.form_field_options" :key="key">
                    {{ option.option_label }}
                  </option>
                </select>
                <p class="typo__p" v-if="submitStatus === 'ERROR'">Please select salutation.</p>
                <p class="typo__p" v-if="submitStatus === 'PENDING'">Sending...</p>
              </div>
              <div class="col-12 mt-2 d-flex justify-content-between">
                <button @click.prevent="prev" class="btn btn-outline-primary">Previous</button>
                <button @submit.prevent="next" class="btn btn-outline-primary">Next</button>
              </div>
            </div>
          </div>
          <!--          TODO
                    <div class="form-group container" @keyup.enter="next" @keyup.left="prev"
                         v-if="activeStep.form.field_type_id === 2">
                      <div class="row">
                        <div class="col-12">

                          <p class="typo__p" v-if="submitStatus === 'ERROR'">Please fill the form correctly.</p>
                          <p class="typo__p" v-if="submitStatus === 'PENDING'">Sending...</p>
                        </div>
                        <div class="col-12 mt-2 d-flex justify-content-between">
                          <button @click.prevent="prev" class="btn btn-outline-primary">Previous</button>
                          <button @submit.prevent="next" class="btn btn-outline-primary">Next</button>
                        </div>
                      </div>
                    </div>
                   -->
          <div class="form-group container" @keyup.enter.prevent="submitForm" @keyup.left="prev"
               v-if="activeStep.form.field_type_id === 4 && activeStep.form.extra.field_type === 'legacy'">
            <div class="row">
              <div class="col-12">
                <div class="form-check form-check-inline">
                  <label for="last" class="form-check-label">{{ activeStep.form.label }}
                    <p v-if="activeStep.form.is_required" class="text-danger d-inline">*</p>
                  </label>
                  <input type="checkbox" id="last" class="form-check-input"
                         :value="activeStep.form.form_field_options[0].option_value"
                         v-model="forms[activeStep.form.unique_id]">
                </div>
                <p class="typo__p font-weight-bold text-danger" v-if="submitStatus === 'ERROR'">You have to confirm
                  that.</p>
                <p class="typo__p font-weight-bold text-info" v-if="submitStatus === 'PENDING'">Sending...</p>
              </div>
              <div class="col-12 mt-2 d-flex justify-content-between">
                <button @click.prevent="prev" class="btn btn-outline-primary">Previous</button>
                <button @click.prevent="submitForm" class="btn btn-outline-primary" type="submit">Submit form</button>
              </div>
            </div>
          </div>
        </form>
      </div>
    </div>
    <GoodbyeMessage @submitOnGoodbye="SubmitOnGoodbye" v-if="goodbyeExist === true"></GoodbyeMessage>
  </div>
</template>

<script>
import formData from '../../form_data.json';
import WelcomeMessage from "@/components/WelcomeMessage";
import GoodbyeMessage from "@/components/GoodbyeMessage";
import router from "@/router";


export default {
  name: "Fields",
  components: {
    WelcomeMessage,
    GoodbyeMessage,
  },
  data() {
    return {
      forms: {},
      errors: [],
      step: 1,
      form_data: formData,
      activeStep: {stepId: 0, form: formData.data[0]},
      submitStatus: null,
      welcomeExist: true,
      goodbyeExist: false,
    }
  },
  mounted() {
    this.setForms()
    this.setWelcomeExist()
  },
  methods: {
    // Welcome Message Controls
    setWelcomeExist() {
      this.welcomeExist = formData.options.welcome_message.title != null;
    },
    goToForm() {
      return this.welcomeExist = false
    },

    // Goodbye Message Controls - (Look submitForm())
    SubmitOnGoodbye() {
      console.log(this.forms);
      this.successExist()
      setTimeout(() => {
        this.goodbyeExist = false
        this.redirectExist()
      }, 1000)
    },

    // Setting Up Data - Data Reservation
    setForms() {
      const formsData = this.forms
      $.each(formData.data, (key, value) => {
        this.$set(formsData, value.unique_id, [])
      })
      console.log(formsData)
      return formsData
    },

    // Buttons - Submissions
    prev() {
      this.activeStep.stepId--;
      this.activeStep.form = this.form_data.data[this.activeStep.stepId];
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
            this.activeStep.form = this.form_data.data[this.activeStep.stepId];
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
    submitForm() {
      if (this.isValid()) {
        if (this.form_data.options.goodbye_message.title == null) {   // Goodbye Message Control
          console.log(this.forms)
          this.submitStatus = 'PENDING'
          this.successExist()
          setTimeout(() => {
            this.submitStatus = 'OK'
            this.redirectExist()
          }, 1000)
        } else {
          return this.goodbyeExist = true
        }
      } else {
        this.submitStatus = 'ERROR'
        console.log("Invalid")
      }
    },

    // Success Message Control - On Submit
    successExist() {
      if (this.form_data.success_message == null) {
        alert("SUBMITTED")
      } else {
        alert(this.form_data.success_message)
      }
    },

    // Redirect Link Control - On Submit
    redirectExist() {
      if (this.form_data.redirect_url == null) {
        router.go(0)
      } else {
        window.open(this.form_data.redirect_url, '_self')
      }
    },

    // Validations
    validEmail(email) {
      const re = /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
      return re.test(email);
    },
    isValid() {
      return !this.activeStep.form.is_required || (this.forms[this.activeStep.form.unique_id] && this.forms[this.activeStep.form.unique_id].length);
    },
  },

  computed: {
    // Maximum Value For Progress Bar
    formLength() {
      const maxValue = Object.keys(this.forms).length
      return maxValue
    }
  }
}
</script>

<style scoped>

</style>
