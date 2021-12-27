<template>
  <div class="col-12">
    <label for="number"><span class="text-muted">{{ activeStep.stepId + 1 }}&rarr; </span><span
        class="font-weight-bold">{{ activeStep.form.label }}</span>
      <p v-if="activeStep.form.is_required" class="text-danger d-inline">*</p>
    </label>
    <input type="number" class="form-control ml-4" id="number" v-model="forms[activeStep.form.unique_id]">
    <p class="typo__p" v-if="submitStatus === 'PENDING'">Sending...</p>
  </div>
</template>

<script>
export default {
  name: "Number",
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