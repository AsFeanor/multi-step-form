<template>
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
</template>

<script>
export default {
  name: "Radio",
  props: {
    activeStep: {
      required: true,
    },
    forms: {
      required: true,
    },
    submitStatus: {
      required: true,
    }
  }
}
</script>

<style scoped>

</style>