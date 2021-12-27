<template>
  <div class="container-fluid mt-5 pt-5">
    <WelcomeMessage @goToForm="goToForm" v-if="welcomeExist"></WelcomeMessage>
    <pre>{{forms}}</pre>
    <div v-if="welcomeExist === false && goodbyeExist === false">
      <form id="myForm" @submit.prevent="next" @keydown.enter.prevent="next">
        <div class="row justify-content-center mt-5 ">
          <div class="col-7">
            <div class="form-group container" v-if="activeStep.form.field_type_id === 7">
              <div class="row">
                <email :activeStep="activeStep" :forms="forms" :submitStatus="submitStatus"/>
                <form-control-buttons @next="next"/>
              </div>
            </div>
            <div class="form-group container" @keyup.left="prev"
                 v-if="activeStep.form.field_type_id === 3">
              <div class="row">
                <selectbox :submit-status="submitStatus" :forms="forms" :active-step="activeStep"/>
                <form-control-buttons @next="next"/>
              </div>
            </div>
            <div class="form-group container" @keyup.left="prev"
                 v-if="activeStep.form.field_type_id === 13">
              <div class="row">
                <rating :submit-status="submitStatus" :forms="forms" :active-step="activeStep"/>
                <form-control-buttons @next="next"/>
              </div>
            </div>
            <div class="form-group container" @keyup.left="prev"
                 v-if="activeStep.form.field_type_id === 4 && activeStep.form.extra.length === 0">
              <div class="row">
                <checkbox :submit-status="submitStatus" :forms="forms" :active-step="activeStep"/>
                <form-control-buttons @next="next"/>
              </div>
            </div>
            <div class="form-group container" @keyup.left="prev"
                 v-if="activeStep.form.field_type_id ===6">
              <div class="row">
                <date-picker :submit-status="submitStatus" :forms="forms" :active-step="activeStep"/>
                <form-control-buttons @next="next"/>
              </div>
            </div>
            <div class="form-group container" @keyup.left="prev"
                 v-if="activeStep.form.field_type_id === 8">
              <div class="row">
                <phone :submit-status="submitStatus" :forms="forms" :active-step="activeStep"/>
                <form-control-buttons @next="next"/>
              </div>
            </div>
            <div class="form-group container" @keyup.left="prev"
                 v-if="activeStep.form.field_type_id === 23 || activeStep.form.field_type_id === 24">
              <div class="row">
                <name-surname :submit-status="submitStatus" :forms="forms" :active-step="activeStep"/>
                <form-control-buttons @next="next"/>
              </div>
            </div>
            <div class="form-group container" @keydown.enter="next" @keyup.left="prev"
                 v-if="activeStep.form.field_type_id === 2">
              <div class="row">
                <number :submit-status="submitStatus" :forms="forms" :active-step="activeStep"/>
                <form-control-buttons @next="next"/>
              </div>
            </div>
            <div class="form-group container" @keyup.left="prev"
                 v-if="activeStep.form.field_type_id === 5">
              <div class="row">
                <radio :submit-status="submitStatus" :forms="forms" :active-step="activeStep"/>
                <form-control-buttons @next="next"/>
              </div>
            </div>
            <div class="form-group container" @keyup.left="prev"
                 v-if="activeStep.form.field_type_id === 9">
              <div class="row">
                <text-area :submit-status="submitStatus" :forms="forms" :active-step="activeStep"/>
                <form-control-buttons @next="next"/>
              </div>
            </div>
            <div class="form-group container" @keyup.left="prev"
                 v-if="activeStep.form.field_type_id === 26">
              <div class="row">
                <salutation :submit-status="submitStatus" :forms="forms" :active-step="activeStep"/>
                <form-control-buttons @next="next"/>
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
import FormControlButtons from "@/components/FormControlButtons";
import Email from "@/components/Field Type/Email";
import Selectbox from "@/components/Field Type/Selectbox";
import Rating from "@/components/Field Type/Rating";
import Checkbox from "@/components/Field Type/Checkbox";
import DatePicker from "@/components/Field Type/DatePicker";
import Phone from "@/components/Field Type/Phone";
import NameSurname from "@/components/Field Type/NameSurname";
import Number from "@/components/Field Type/Number";
import Radio from "@/components/Field Type/Radio";
import TextArea from "@/components/Field Type/TextArea";
import Salutation from "@/components/Field Type/Salutation";
import router from "@/router";
import axios from 'axios';
import Swal from 'sweetalert2'


export default {
  name: "Fields",
  components: {
    WelcomeMessage,
    GoodbyeMessage,
    Rating,
    FormControlButtons,
    Email,
    Selectbox,
    Checkbox,
    DatePicker,
    Phone,
    NameSurname,
    Number,
    Radio,
    TextArea,
    Salutation,
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

    // Setting Up Data - Data Reservation
    setForms() {
      const formsData = this.forms
      $.each(formData.data, (key, value) => {
        this.$set(formsData, value.unique_id, [])
      })
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
  }
}
</script>

<style scoped>
::v-deep .form-control {
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
