<template>
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
</template>

<script>
export default {
  name: "Rating",
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
  },
  computed: {
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

</style>