<template>
  <div class="col-12">
    <label for="email">{{ activeStep.form.label }}
      <p v-if="activeStep.form.is_required" class="text-danger d-inline">*</p>
    </label>
    <input type="email" class="form-control" id="email" v-model="forms[activeStep.form.unique_id]">
    <p class="typo__p" v-if="submitStatus === 'PENDING'">Sending...</p>
  </div>
</template>

<script>
export default {
  name: "Email"
}
</script>

<style scoped>

</style>
