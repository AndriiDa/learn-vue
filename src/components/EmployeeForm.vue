<template>
  <div id="employee-form">
    <form @submit.prevent="handleSubmit">
      <label>Employee name</label>
      <input
        ref="first"
        type="text"
        :class="{ 'has-error': submitting && invalidName }"
        v-model="employee.name"
        @focus="clearStatus"
        @keypress="clearStatus"
      >
      <label>Employee Email</label>
      <input
        type="text"
        :class="{ 'has-error': submitting && invalidEmail }"
        v-model="employee.email"
        @focus="clearStatus"
      >
      <p v-if="error && submitting" class="error-message">❗Please fill out all required fields</p>
      <p v-if="success" class="success-message">✅ Employee successfully added</p>
      <button class="button--grey">Add Employee</button>
    </form>
  </div>
</template>

<script>
export default {
  name: "EmployeeForm",
  data() {
    return {
      submitting: false,
      error: false,
      success: false,
      employee: {
        name: "",
        email: ""
      }
    };
  },
  methods: {
    handleSubmit() {
      this.submitting = true;
      this.clearStatus();

      if (this.invalidName || this.invalidEmail) {
        this.error = true;
        return;
      }

      this.$emit('add:employee', this.employee)
this.$refs.first.focus()
      this.employee = {
        name: "",
        email: ""
      };
      this.error = false;
      this.success = true;
      this.submitting = false;
    },

    clearStatus() {
      this.success = false;
      this.error = false;
    }
  },
  computed: {
    invalidName() {
      return this.employee.name === "";
    },

    invalidEmail() {
      return this.employee.email === "";
    }
  }
};
</script>

<style scoped>
form {
  display: flex;
  justify-content: center;
  flex-direction: column;
  margin-bottom: 2rem;
  border: 1px;
  border-radius: 2px;
  padding: 0.75rem;
  outline: none;
  background: rgb(230, 223, 223);
  margin-bottom: 0.5rem;
  font-size: 1rem;
  width: 50%;
  max-width: 50%;
  line-height: 1;
}

label,
input {
  font-weight: 600;
  max-width: 100%;
  display: block;
  margin: 1rem 0 0.5rem;
}

[class*="-message"] {
  font-weight: 500;
}

.error-message {
  color: #d33c40;
}

.success-message {
  color: #32a95d;
}
</style>