<template>
  <div>
    <div class="container">
      <div class="row">
        <div class="rating">
          <label for="star5">
            <input id="star5" name="rating" type="radio" value="5"/>
          </label>
          <input id="star4" name="rating" type="radio" value="4"/>
          <label for="star4"></label>
          <input id="star3" name="rating" type="radio" value="3"/>
          <label for="star3"></label>
          <input id="star2" name="rating" type="radio" value="2"/>
          <label for="star2"></label>
          <input id="star1" name="rating" type="radio" value="1"/>
          <label for="star1"></label>
        </div>
      </div>
    </div>
  </div>
</template>
​
<script>
export default {
  name: 'Rating',
  props: {
    formFields: {
      type: Array,
    },
    value: {
      type: Number,
      default: 2,
    },
    unique_id: {
      type: String,
      default: 'FormFields',
    },
  },
  data() {
    return {
      selected_value: null,
      temp_value: null,
    };
  },
  methods: {
    star_over(index) {
      const self = this
      if (!self.disabled) {
        self.temp_value = self.value;
        self.value = index;
        return self.value;
      }
      return null;
    },
    star_out() {
      const self = this
      if (!self.disabled) {
        self.value = self.temp_value;
        return self.value;
      }
      return null;
    },
    set_value(value) {
      const self = this
      if (!self.disabled) {
        self.temp_value = value;
        self.value = value;
        return self.value;
      }
      return null;
    },
  }
};
</script>


<style lang="css" scoped>
.rating {
  float: left;
}

/* :not(:checked) is a filter, so that browsers that don’t support :checked don’t
  follow these rules. Every browser that supports :checked also supports :not(), so
  it doesn’t make the test unnecessarily selective */
.rating:not(:checked) > input {
  position: absolute;
  top: -9999px;
  clip: rect(0, 0, 0, 0);
}

.rating:not(:checked) > label {
  float: right;
  width: 1em;
  /* padding:0 .1em; */
  overflow: hidden;
  white-space: nowrap;
  cursor: pointer;
  font-size: 300%;
  /* line-height:1.2; */
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
