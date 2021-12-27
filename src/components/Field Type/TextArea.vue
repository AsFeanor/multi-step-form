<template>
  <div class="col-12">
    <label for="textarea"><span class="text-muted">{{ activeStep.stepId + 1 }}&rarr; </span><span
        class="font-weight-bold">{{ activeStep.form.label }}</span>
      <p v-if="activeStep.form.is_required" class="text-danger d-inline">*</p>
    </label>
    <textarea id="textarea" class="form-control ml-4" cols="30" rows="10"
              v-model="forms[activeStep.form.unique_id]"></textarea>
    <p class="typo__p" v-if="submitStatus === 'PENDING'">Sending...</p>
  </div>
</template>

<script>
export default {
  name: "TextArea",
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