<template>
  <div class="container-fluid mt-5 pt-5">
    <WelcomeMessage @goToForm="goToForm" v-if="welcomeExist"></WelcomeMessage>
    <div v-if="welcomeExist === false && goodbyeExist === false">
      <form id="myForm" @submit.prevent="next" @keydown.enter.prevent="next">
        <div class="row justify-content-center mt-5 ">
          <div class="col-7">
            <div class="form-group container" v-if="activeStep.form.field_type_id === 7">
              <div class="row">
                <div class="col-12">
                  <label for="email"><span class="text-muted">{{ activeStep.stepId + 1 }}&rarr; </span>
                    <span class="font-weight-bold">{{ activeStep.form.label }}</span>
                    <p v-if="activeStep.form.is_required" class="text-danger d-inline">*</p>
                  </label>
                  <input type="email" class="form-control ml-4" id="email" v-model="forms[activeStep.form.unique_id]">
                  <p class="typo__p" v-if="submitStatus === 'PENDING'">Sending...</p>
                </div>
                <div class="col-12 mt-3 ml-4 d-inline-flex">
                  <button type="submit" @submit.prevent="next" class="btn btn-dark ok-button">Ok</button>
                  <p class="mt-2 ml-2"><span class="text-muted">Press </span>Enter</p>
                </div>
              </div>
            </div>
            <div class="form-group container" @keyup.left="prev"
                 v-if="activeStep.form.field_type_id === 3">
              <div class="row">
                <div class="col-12">
                  <label for="yes-no"><span class="text-muted">{{ activeStep.stepId + 1 }}&rarr; </span>
                    <span class="font-weight-bold">{{ activeStep.form.label }}</span>
                    <p v-if="activeStep.form.is_required" class="text-danger d-inline">*</p>
                  </label>
                  <select class="form-control ml-4" id="yes-no" v-model="forms[activeStep.form.unique_id]">
                    <option v-for="(option, key) in activeStep.form.form_field_options" :key="key">
                      {{ option.option_label }}
                    </option>
                  </select>
                  <p class="typo__p" v-if="submitStatus === 'PENDING'">Sending...</p>
                </div>
                <div class="col-12 mt-3 ml-4 d-inline-flex">
                  <button type="submit" @submit.prevent="next" class="btn btn-dark ok-button">Ok</button>
                  <p class="mt-2 ml-2"><span class="text-muted">Press </span>Enter</p>
                </div>
              </div>
            </div>
            <div class="form-group container" @keyup.left="prev"
                 v-if="activeStep.form.field_type_id === 13">
              <div class="row">
                <div class="col-12 d-flex justify-content-center"><span class="text-muted">{{ activeStep.stepId + 1 }}&rarr; </span>
                  <span class="font-weight-bold">{{ activeStep.form.label }}</span>
                  <p v-if="activeStep.form.is_required" class="text-danger d-inline">*</p>
                  <div class="rating">
                    <template v-for="option in reverseItems">
                      <input :id="option.option_label" name="rating" type="radio" :value="option.option_value"
                             v-model="forms[activeStep.form.unique_id]"/>
                      <label :for="option.option_label" class=""></label>
                    </template>
                  </div>
                  <p class="typo__p" v-if="submitStatus === 'PENDING'">Sending...</p>
                </div>
                <div class="col-12 mt-3 ml-4 d-inline-flex">
                  <button type="submit" @submit.prevent="next" class="btn btn-dark ok-button">Ok</button>
                  <p class="mt-2 ml-2"><span class="text-muted">Press </span>Enter</p>
                </div>
              </div>
            </div>
            <div class="form-group container" @keyup.left="prev"
                 v-if="activeStep.form.field_type_id === 4 && activeStep.form.extra.length === 0">
              <div class="row">
                <div class="col-12">
                  <div><span class="text-muted">{{ activeStep.stepId + 1 }}&rarr; </span><span class="font-weight-bold">{{
                      activeStep.form.label
                    }}</span>
                    <p v-if="activeStep.form.is_required" class="text-danger d-inline">*</p>
                  </div>
                  <div class="form-check form-check-inline d-flex justify-content-center"
                       v-for="(option, key) in activeStep.form.form_field_options" :key="key">
                    <input type="checkbox" class="form-check-input" :id="option.form_field_option_id"
                           :value="option.option_value" v-model="forms[activeStep.form.unique_id]">
                    <label :for="option.form_field_option_id" class="form-check-label">{{ option.option_label }}</label>
                  </div>
                  <p class="typo__p" v-if="submitStatus === 'PENDING'">Sending...</p>
                </div>
                <div class="col-12 mt-3 ml-4 d-inline-flex">
                  <button type="submit" @submit.prevent="next" class="btn btn-dark ok-button">Ok</button>
                  <p class="mt-2 ml-2"><span class="text-muted">Press </span>Enter</p>
                </div>
              </div>
            </div>
            <div class="form-group container" @keyup.left="prev"
                 v-if="activeStep.form.field_type_id ===6">
              <div class="row">
                <div class="col-12 d-flex justify-content-center">
                  <label for="time-start" class="placeholder"><span class="text-muted">{{
                      activeStep.stepId + 1
                    }}&rarr; </span><span class="font-weight-bold">{{ activeStep.form.label }}</span>
                    <p v-if="activeStep.form.is_required" class="text-danger d-inline">*</p>
                  </label>
                </div>
                <div class="col-12 d-flex justify-content-center">
                  <input id="time-start" class="input" type="datetime-local" placeholder=" "
                         v-model="forms[activeStep.form.unique_id]"/>
                </div>
                <p class="typo__p" v-if="submitStatus === 'PENDING'">Sending...</p>
                <div class="col-12 mt-3 ml-4 d-inline-flex">
                  <button type="submit" @submit.prevent="next" class="btn btn-dark ok-button">Ok</button>
                  <p class="mt-2 ml-2"><span class="text-muted">Press </span>Enter</p>
                </div>
              </div>
            </div>
            <div class="form-group container" @keyup.left="prev"
                 v-if="activeStep.form.field_type_id === 8">
              <div class="row">
                <div class="col-12">
                  <label for="phone"><span class="text-muted">{{ activeStep.stepId + 1 }}&rarr; </span><span
                      class="font-weight-bold">{{ activeStep.form.label }}</span>
                    <p v-if="activeStep.form.is_required" class="text-danger d-inline">*</p>
                  </label>
                  <input type="tel" class="form-control ml-4" id="phone" v-model="forms[activeStep.form.unique_id]">
                  <p class="typo__p" v-if="submitStatus === 'PENDING'">Sending...</p>
                </div>
                <div class="col-12 mt-3 ml-4 d-inline-flex">
                  <button type="submit" @submit.prevent="next" class="btn btn-dark ok-button">Ok</button>
                  <p class="mt-2 ml-2"><span class="text-muted">Press </span>Enter</p>
                </div>
              </div>
            </div>
            <div class="form-group container" @keyup.left="prev"
                 v-if="activeStep.form.field_type_id === 23">
              <div class="row">
                <div class="col-12">
                  <label for="name"><span class="text-muted">{{ activeStep.stepId + 1 }}&rarr; </span><span
                      class="font-weight-bold">{{ activeStep.form.label }}</span>
                    <p v-if="activeStep.form.is_required" class="text-danger d-inline">*</p>
                  </label>
                  <input type="text" class="form-control ml-4" id="name" v-model="forms[activeStep.form.unique_id]"><br>
                  <p class="typo__p" v-if="submitStatus === 'PENDING'">Sending...</p>
                </div>
                <div class="col-12 mt-3 ml-4 d-inline-flex">
                  <button type="submit" @submit.prevent="next" class="btn btn-dark ok-button">Ok</button>
                  <p class="mt-2 ml-2"><span class="text-muted">Press </span>Enter</p>
                </div>
              </div>
            </div>
            <div class="form-group container" @keyup.left="prev"
                 v-if="activeStep.form.field_type_id === 24">
              <div class="row">
                <div class="col-12">
                  <label for="surname"><span class="text-muted">{{ activeStep.stepId + 1 }}&rarr; </span><span
                      class="font-weight-bold">{{ activeStep.form.label }}</span>
                    <p v-if="activeStep.form.is_required" class="text-danger d-inline">*</p>
                  </label>
                  <input type="text" class="form-control ml-4" id="surname" v-model="forms[activeStep.form.unique_id]">
                  <p class="typo__p" v-if="submitStatus === 'PENDING'">Sending...</p>
                </div>
                <div class="col-12 mt-3 ml-4 d-inline-flex">
                  <button type="submit" @submit.prevent="next" class="btn btn-dark ok-button">Ok</button>
                  <p class="mt-2 ml-2"><span class="text-muted">Press </span>Enter</p>
                </div>
              </div>
            </div>
            <div class="form-group container" @keydown.enter="next" @keyup.left="prev"
                 v-if="activeStep.form.field_type_id === 2">
              <div class="row">
                <div class="col-12">
                  <label for="number"><span class="text-muted">{{ activeStep.stepId + 1 }}&rarr; </span><span
                      class="font-weight-bold">{{ activeStep.form.label }}</span>
                    <p v-if="activeStep.form.is_required" class="text-danger d-inline">*</p>
                  </label>
                  <input type="number" class="form-control ml-4" id="number" v-model="forms[activeStep.form.unique_id]">
                  <p class="typo__p" v-if="submitStatus === 'PENDING'">Sending...</p>
                </div>
                <div class="col-12 mt-3 ml-4 d-inline-flex">
                  <button type="submit" @submit.prevent="next" class="btn btn-dark ok-button">Ok</button>
                  <p class="mt-2 ml-2"><span class="text-muted">Press </span>Enter</p>
                </div>
              </div>
            </div>
            <div class="form-group container" @keyup.left="prev"
                 v-if="activeStep.form.field_type_id === 5">
              <div class="row">
                <div class="col-12">
                  <div><span class="text-muted">{{ activeStep.stepId + 1 }}&rarr; </span><span class="font-weight-bold">{{
                      activeStep.form.label
                    }}</span>
                    <p v-if="activeStep.form.is_required" class="text-danger d-inline">*</p>
                  </div>
                  <div class="form-check form-check-inline d-flex justify-content-center"
                       v-for="(option, key) in activeStep.form.form_field_options" :key="key">
                    <input type="radio" class="form-check-input" :id="option.form_field_option_id"
                           :value="option.option_value" v-model="forms[activeStep.form.unique_id]">
                    <label :for="option.form_field_option_id" class="form-check-label">{{ option.option_label }}</label>
                  </div>
                  <p class="typo__p" v-if="submitStatus === 'PENDING'">Sending...</p>
                </div>
                <div class="col-12 mt-3 ml-4 d-inline-flex">
                  <button type="submit" @submit.prevent="next" class="btn btn-dark ok-button">Ok</button>
                  <p class="mt-2 ml-2"><span class="text-muted">Press </span>Enter</p>
                </div>
              </div>
            </div>
            <div class="form-group container" @keyup.left="prev"
                 v-if="activeStep.form.field_type_id === 9">
              <div class="row">
                <div class="col-12">
                  <label for="textarea"><span class="text-muted">{{ activeStep.stepId + 1 }}&rarr; </span><span
                      class="font-weight-bold">{{ activeStep.form.label }}</span>
                    <p v-if="activeStep.form.is_required" class="text-danger d-inline">*</p>
                  </label>
                  <textarea id="textarea" class="form-control ml-4" cols="30" rows="10"
                            v-model="forms[activeStep.form.unique_id]"></textarea>
                  <p class="typo__p" v-if="submitStatus === 'PENDING'">Sending...</p>
                </div>
                <div class="col-12 mt-3 ml-4 d-inline-flex">
                  <button type="submit" @submit.prevent="next" class="btn btn-dark ok-button">Ok</button>
                  <p class="mt-2 ml-2"><span class="text-muted">Press </span>Enter</p>
                </div>
              </div>
            </div>
            <div class="form-group container" @keyup.left="prev"
                 v-if="activeStep.form.field_type_id === 11">
              <div class="row">
                <div class="col-12">
                  <div><span class="text-muted">{{ activeStep.stepId + 1 }}&rarr; </span><span class="font-weight-bold">{{
                      activeStep.form.label
                    }}</span>
                    <p v-if="activeStep.form.is_required" class="text-danger d-inline">*</p>
                  </div>
                  <div class="form-check form-check-inline d-flex justify-content-center"
                       v-for="(option, key) in activeStep.form.form_field_options" :key="key">
                    <input type="radio" class="form-check-input" :id="option.form_field_option_id"
                           :value="option.option_value" v-model="forms[activeStep.form.unique_id]">
                    <label :for="option.form_field_option_id" class="form-check-label">{{ option.option_label }}</label>
                  </div>
                  <p class="typo__p" v-if="submitStatus === 'PENDING'">Sending...</p>
                </div>
                <div class="col-12 mt-3 ml-4 d-inline-flex">
                  <button type="submit" @submit.prevent="next" class="btn btn-dark ok-button">Ok</button>
                  <p class="mt-2 ml-2"><span class="text-muted">Press </span>Enter</p>
                </div>
              </div>
            </div>
            <div class="form-group container" @keyup.left="prev"
                 v-if="activeStep.form.field_type_id === 26">
              <div class="row">
                <div class="col-12">
                  <label for="salutation"><span class="text-muted">{{ activeStep.stepId + 1 }}&rarr; </span><span
                      class="font-weight-bold">{{ activeStep.form.label }}</span>
                    <p v-if="activeStep.form.is_required" class="text-danger d-inline">*</p>
                  </label>
                  <select class="form-control ml-4" id="salutation" v-model="forms[activeStep.form.unique_id]">
                    <option v-for="(option, key) in activeStep.form.form_field_options" :key="key">
                      {{ option.option_label }}
                    </option>
                  </select>
                  <p class="typo__p" v-if="submitStatus === 'PENDING'">Sending...</p>
                </div>
                <div class="col-12 mt-3 ml-4 d-inline-flex">
                  <button type="submit" @submit.prevent="next" class="btn btn-dark ok-button">Ok</button>
                  <p class="mt-2 ml-2"><span class="text-muted">Press </span>Enter</p>
                </div>
              </div>
            </div>
            <div class="form-group container" @keyup.left="prev"
                 v-if="activeStep.form.field_type_id === 22">
              <div class="row">
                <div class="col-12">
                  <label for="address"><span class="text-muted">{{ activeStep.stepId + 1 }}&rarr; </span><span
                      class="font-weight-bold">{{ activeStep.form.label }}</span></label>
                  <div class="input-group">
                    <input type="text" id="address" class="form-control" style="width: 100% !important;"
                           :placeholder="activeStep.form.placeholder" :aria-label="activeStep.form.placeholder"
                           aria-describedby="basic-addon2"
                           v-model="forms[activeStep.form.unique_id]">
                    <div class="input-group-append">
                      <button @click="getAddresses" class="btn btn-outline-dark" type="button">Find <strong> > </strong>
                      </button>
                    </div>
                  </div>
                  <div>
                    <select class="custom-select" size="5" v-if="address" v-model="forms[activeStep.form.unique_id]">
                      <option @click="setAddress(address, activeStep.form.unique_id)" v-for="address in addresses">
                        {{ address.summaryline }}
                      </option>
                    </select>
                    <p class="typo__p" v-if="submitStatus === 'PENDING'">Sending...</p>
                  </div>
                </div>
                <div class="col-12 mt-3 ml-4 d-inline-flex">
                  <button type="submit" @submit.prevent="next" class="btn btn-dark ok-button">Ok</button>
                  <p class="mt-2 ml-2"><span class="text-muted">Press </span>Enter</p>
                </div>
              </div>
            </div>
            <div class="form-group container" @keyup.enter.prevent="submitForm" @keyup.left="prev"
                 v-if="activeStep.form.field_type_id === 4 && activeStep.form.extra.field_type === 'legacy'">
              <div class="row">
                <div class="col-12">
                  <div class="form-check form-check-inline">
                    <label for="last" class="form-check-label"><span class="text-muted">{{ activeStep.stepId + 1 }}&rarr; </span>{{
                        activeStep.form.label
                      }}
                      <p v-if="activeStep.form.is_required" class="text-danger d-inline">*</p>
                    </label>
                    <input type="checkbox" id="last" class="form-check-input"
                           :value="activeStep.form.form_field_options[0].option_value"
                           v-model="forms[activeStep.form.unique_id]">
                  </div>
                  <p class="typo__p font-weight-bold text-info" v-if="submitStatus === 'PENDING'">Sending...</p>
                </div>
                <div class="col-12 mt-3 ml-4 d-inline-flex">
                  <button type="submit" @click="submitForm" class="btn btn-dark ok-button">Submit</button>
                  <p class="mt-2 ml-2"><span class="text-muted">Press </span>Enter</p>
                </div>
              </div>
            </div>
          </div>
        </div>
        <div class="row justify-content-end mt-5 pt-5">
          <div class="col-2 mb-5 mt-5 pt-5 pl-0">
            <div class="row">
              {{ activeStep.stepId + 1 }} of {{ formLength }} answered
            </div>
            <div class="row">
              <div class="progress" style="height: 3px">
                <div class="progress-bar bg-dark" role="progressbar"
                     :style="{ width: ((activeStep.stepId + 1)/(formLength/100)) + '%' }"
                     :aria-valuenow="activeStep.stepId + 1" aria-valuemin="0"
                     :aria-valuemax="formLength">
                </div>
              </div>
            </div>

          </div>
          <div class="col-1 mb-5 mt-5 pt-5 pl-0">
            <button v-if="activeStep.stepId !== 0" @click.prevent="prev" class="btn btn-dark ml-2">&darr;</button>
            <button v-if="activeStep.stepId + 1 !== formLength" @submit.prevent="next" class="btn btn-dark ml-1">
              &uarr;
            </button>
          </div>
        </div>
      </form>
      <!--      TEST-->
      <!--      <div class="col-6">-->
      <!--        <pre>{{ forms }}</pre>-->
      <!--        <pre>{{ addresses }}</pre>-->
      <!--      </div>-->
    </div>
    <GoodbyeMessage v-if="goodbyeExist"></GoodbyeMessage>
  </div>
</template>

<script>
import formData from '../../form_data.json';
import WelcomeMessage from "@/components/WelcomeMessage";
import GoodbyeMessage from "@/components/GoodbyeMessage";
import Email from "@/components/Field Type/Email";
import Rating from "@/components/Rating";
import router from "@/router";
import axios from 'axios';
import Swal from 'sweetalert2'


export default {
  name: "Fields",
  components: {
    WelcomeMessage,
    GoodbyeMessage,
    Rating,
    Email
  },
  data() {
    return {
      forms: {},
      errors: [],
      step: 1,
      form_data: formData,
      activeStep: {stepId: 0, form: formData.data[0]},
      address: false,
      addresses: [],
      submitStatus: null,
      welcomeExist: true,
      goodbyeExist: false,
    }
  },
  created() {
    // Setting Up Page Title
    window.document.title = formData.options.page.page_title
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

    // Setting Address
    setAddress(address, uniqueId) {
      let Address1;
      if (address.buildingname) {
        Address1 = address.buildingname
      } else {
        Address1 = address.number
      }
      this.forms[uniqueId] = {
        address_1: Address1,
        address_2: address.street,
        address_3: address.dependentlocality ? address.dependentlocality : " ",
        address_4: address.posttown,
        address_5: address.county,
        country_id: 1,
        display_name: address.summaryline,
        postcode: address.postcode,
      }
    },

    // Setting Up Data - Data Reservation
    setForms() {
      const formsData = this.forms
      $.each(formData.data, (key, value) => {
        this.$set(formsData, value.unique_id, [])
      })
      // console.log(formsData)
      return formsData
    },

    // Welcome Message Controls
    setWelcomeExist() {
      this.welcomeExist = formData.options.welcome_message.title != null;
    },
    goToForm() {
      return this.welcomeExist = false
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
        this.requiredAlert()
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
          setTimeout(() => {
            this.submitStatus = 'OK'
          }, 1000)
        } else {
          return this.goodbyeExist = true
        }
      } else {
        this.requiredAlert()
        this.submitStatus = 'ERROR'
        console.log("Invalid")
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
    requiredAlert() {
      const Toast = Swal.mixin({
        toast: true,
        position: 'top-end',
        showConfirmButton: false,
        timer: 2000,
        timerProgressBar: true,
        didOpen: (toast) => {
          toast.addEventListener('mouseenter', Swal.stopTimer)
          toast.addEventListener('mouseleave', Swal.resumeTimer)
        }
      })
      Toast.fire({
        icon: 'error',
        title: 'This field is required!'
      })
    }
  },

  computed: {
    // Maximum Value Of Form Data
    formLength() {
      return Object.keys(this.forms).length
    },

    // Reversed Array for Rating
    reverseItems() {
      return this.activeStep.form.form_field_options.slice().reverse();
    }
  }
}
</script>

<style scoped>
.rating {
  transform: rotateY(180deg);
}

.rating:not(:checked) > input {
  position: absolute;
  clip: rect(0, 0, 0, 0);
}

.rating:not(:checked) > label {
  width: 1em;
  overflow: hidden;
  white-space: nowrap;
  cursor: pointer;
  font-size: 300%;
  color: #ddd;
}

.rating:not(:checked) > label:before {
  content: '★ ';
}

.rating > input:checked ~ label {
  color: #FFD700;
}

.rating:not(:checked) > label:hover,
.rating:not(:checked) > label:hover ~ label {
  color: #FFD700;
}

.rating > input:checked + label:hover,
.rating > input:checked + label:hover ~ label,
.rating > input:checked ~ label:hover,
.rating > input:checked ~ label:hover ~ label,
.rating > label:hover ~ input:checked ~ label {
  color: #FFD700;
}

.rating > label:active {
  position: relative;
  top: 2px;
  left: 2px;
}

.form-control {
  border-top: none !important;
  border-left: none !important;
  border-right: none !important;
  border-bottom: 1px solid #ced4da !important;
  border-radius: 0 !important;
  width: fill;
}

.progress {
  width: 100%;
  display: inline-flex;
}

.ok-button {
  width: 95px;
  height: 50px;
}
</style>
