<template>
  <form @submit.prevent="submitForm">
    <div
      class="form-control"
      :class="{ invalid: !formWatch.firstName.isValid }"
    >
      <label for="firstname">Firstname</label>
      <input
        type="text"
        id="firstname"
        v-model.trim="formWatch.firstName.val"
      />
      <p v-if="!formWatch.firstName.isValid">Firstname must not be empty.</p>
    </div>
    <div class="form-control" :class="{ invalid: !formWatch.lastName.isValid }">
      <label for="lastname">Lastname</label>
      <input type="text" id="lastname" v-model.trim="formWatch.lastName.val" />
      <p v-if="!formWatch.lastName.isValid">Lastname must not be empty.</p>
    </div>
    <div
      class="form-control"
      :class="{ invalid: !formWatch.description.isValid }"
    >
      <label for="description">Description</label>
      <textarea
        id="description"
        rows="5"
        v-model.trim="formWatch.description.val"
      ></textarea>
      <p v-if="!formWatch.description.isValid">
        Description must not be empty.
      </p>
    </div>
    <div class="form-control" :class="{ invalid: !formWatch.rate.isValid }">
      <label for="rate">Hourly Rate</label>
      <input type="number" id="rate" v-model.number="formWatch.rate.val" />
      <p v-if="!formWatch.rate.isValid">Rate must be greater than 0.</p>
    </div>
    <div class="form-control" :class="{ invalid: !formWatch.areas.isValid }">
      <h3>Areas of Expertise</h3>
      <div>
        <input
          type="checkbox"
          id="frontend"
          value="frontend"
          v-model="formWatch.areas.val"
        />
        <label for="frontend">Frontend Development</label>
      </div>
      <div>
        <input
          type="checkbox"
          id="backend"
          value="backend"
          v-model="formWatch.areas.val"
        />
        <label for="backend">Backend Development</label>
      </div>
      <div>
        <input
          type="checkbox"
          id="career"
          value="career"
          v-model="formWatch.areas.val"
        />
        <label for="career">Career Advisory</label>
      </div>
      <p v-if="!formWatch.areas.isValid">
        At least one expertise must be selected.
      </p>
    </div>
    <p v-if="!formIsValid">Please fix the above errors and submit again.</p>
    <base-button>Register</base-button>
  </form>
</template>

<script>
export default {
  emits: ['save-data'],
  data() {
    return {
      formWatch: {
        firstName: {
          val: '',
          isValid: true,
        },
        lastName: {
          val: '',
          isValid: true,
        },
        description: {
          val: '',
          isValid: true,
        },
        rate: {
          val: null,
          isValid: true,
        },
        areas: {
          val: [],
          isValid: true,
        },
      },
      formIsValid: true,
    };
  },
  watch: {
    'formWatch.firstName.val': {
      handler(newVal) {
        this.formWatch.firstName.isValid = newVal !== '';
      },
    },
    'formWatch.lastName.val': {
      handler(newVal) {
        this.formWatch.lastName.isValid = newVal !== '';
      },
    },
    'formWatch.description.val': {
      handler(newVal) {
        this.formWatch.description.isValid = newVal !== '';
      },
    },
    'formWatch.rate.val': {
      handler(newVal) {
        this.formWatch.rate.isValid = newVal !== null && newVal >= 0;
      },
    },
    'formWatch.areas.val': {
      handler(newVal) {
        this.formWatch.areas.isValid = newVal.length > 0;
      },
    },
  },
  methods: {
    validateForm() {
      this.formIsValid = true;
      if (this.formWatch.firstName.val === '') {
        this.formWatch.firstName.isValid = false;
        this.formIsValid = false;
      }
      if (this.formWatch.lastName.val === '') {
        this.formWatch.lastName.isValid = false;
        this.formIsValid = false;
      }
      if (this.formWatch.description.val === '') {
        this.formWatch.description.isValid = false;
        this.formIsValid = false;
      }
      if (!this.formWatch.rate.val || this.formWatch.rate.val < 0) {
        this.formWatch.rate.isValid = false;
        this.formIsValid = false;
      }
      if (this.formWatch.areas.val.length === 0) {
        this.formWatch.areas.isValid = false;
        this.formIsValid = false;
      }
    },
    submitForm() {
      this.validateForm();

      if (!this.formIsValid) {
        return;
      }

      const formData = {
        first: this.formWatch.firstName.val,
        last: this.formWatch.lastName.val,
        desc: this.formWatch.description.val,
        rate: this.formWatch.rate.val,
        areas: this.formWatch.areas.val,
      };

      this.$emit('save-data', formData);
    },
  },
};
</script>

<style scoped>
.form-control {
  margin: 0.5rem 0;
}

label {
  font-weight: bold;
  display: block;
  margin-bottom: 0.5rem;
}

input[type='checkbox'] + label {
  font-weight: normal;
  display: inline;
  margin: 0 0 0 0.5rem;
}

input,
textarea {
  display: block;
  width: 100%;
  border: 1px solid #ccc;
  font: inherit;
}

input:focus,
textarea:focus {
  background-color: #f0e6fd;
  outline: none;
  border-color: #3d008d;
}

input[type='checkbox'] {
  display: inline;
  width: auto;
  border: none;
}

input[type='checkbox']:focus {
  outline: #3d008d solid 1px;
}

h3 {
  margin: 0.5rem 0;
  font-size: 1rem;
}

.invalid label {
  color: red;
}

.invalid input,
.invalid textarea {
  border: 1px solid red;
}
</style>
