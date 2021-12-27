<template>
  <div class="col-12">
    <label :for="activeStep.form.field_type_id === 23 ? 'name' : 'surname'"><span class="text-muted">{{ activeStep.stepId + 1 }}&rarr; </span><span
        class="font-weight-bold">{{ activeStep.form.label }}</span>
      <p v-if="activeStep.form.is_required" class="text-danger d-inline">*</p>
    </label>
    <input type="text" class="form-control ml-4" :id="activeStep.form.field_type_id === 23 ? 'name' : 'surname'" v-model="forms[activeStep.form.unique_id]"><br>
    <p class="typo__p" v-if="submitStatus === 'PENDING'">Sending...</p>
  </div>
</template>

<script>
export default {
  name: "NameSurname",
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