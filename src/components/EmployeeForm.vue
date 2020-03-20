<template>
  <div id="employee-form">
    <form @submit.prevent="handleSubmit">
      <label>Name</label>
      <input
        ref="first"
        type="text"
        :class="{ 'has-error': submitting && invalidName }"
        v-model="employee.name"
        @focus="clearStatus"
        @keypress="clearStatus"
      />
      <label>Amount to borrow</label>
      <input
        type="number"
        :class="{ 'has-error': submitting && invalidAmount }"
        v-model="employee.amount"
        @focus="clearStatus"
      />
      <label>Interest</label>
      <input
        type="number"
        :class="{ 'has-error': submitting && invalidInterest }"
        v-model="employee.interest"
        @focus="clearStatus"
      />
      <label>Years</label>
      <input
        type="number"
        :class="{ 'has-error': submitting && invalidYears }"
        v-model="employee.years"
        @focus="clearStatus"
      />
      <p v-if="error && submitting" class="error-message">❗Please fill out all required fields</p>
      <p v-if="success" class="success-message">✅ Employee successfully added</p>
      <button>Calculate Mortage</button>
    </form>
  </div>
</template>

<script>
export default {
  name: "employee-form",
  data() {
    return {
      submitting: false,
      error: false,
      success: false,
      employee: {
        name: "",
        amount: "",
        interest: "",
        years: ""
      }
    };
  },
  methods: {
    handleSubmit() {
      this.submitting = true;
      this.clearStatus();

      if (
        this.invalidName ||
        this.invalidAmount ||
        this.invalidInterest ||
        this.invalidYears
      ) {
        this.error = true;
        return;
      }

      this.$emit("add:employee", this.employee);
      this.$refs.first.focus();
      this.employee = {
        name: "",
        amount: "",
        interest: "",
        years: ""
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

    invalidAmount() {
      return this.employee.amount === "";
    },
    invalidInterest() {
      return this.employee.interest === "";
    },
    invalidYears() {
      return this.employee.years === "";
    }
  }
};
</script>

<style scoped>
form {
  margin-bottom: 2rem;
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