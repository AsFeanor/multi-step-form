<template>
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
</template>

<script>
export default {
  name: "Salutation",
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