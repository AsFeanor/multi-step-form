<template>
  <div>
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
  </div>
</template>

<script>
export default {
  name: "DatePicker",
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