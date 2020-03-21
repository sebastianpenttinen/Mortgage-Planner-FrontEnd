<template>
  <div id="mortage-form">
    <form @submit.prevent="handleSubmit">
      <label>Name</label>
      <input
        ref="first"
        type="text"
        :class="{ 'has-error': submitting && invalidName }"
        v-model="mortage.name"
        @focus="clearStatus"
        @keypress="clearStatus"
      />
      <label>Amount to borrow</label>
      <input
        type="number"
        :class="{ 'has-error': submitting && invalidAmount }"
        v-model="mortage.amount"
        @focus="clearStatus"
      />
      <label>Interest</label>
      <input
        type="number"
        :class="{ 'has-error': submitting && invalidInterest }"
        v-model="mortage.interest"
        @focus="clearStatus"
      />
      <label>Years</label>
      <input
        type="number"
        :class="{ 'has-error': submitting && invalidYears }"
        v-model="mortage.years"
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
  name: "mortage-form",
  data() {
    return {
      submitting: false,
      error: false,
      success: false,
      mortage: {
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

      this.$emit("add:mortage", this.mortage);
      this.$refs.first.focus();
      this.mortage = {
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
      return this.mortage.name === "";
    },

    invalidAmount() {
      return this.mortage.amount === "";
    },
    invalidInterest() {
      return this.mortage.interest === "";
    },
    invalidYears() {
      return this.mortage.years === "";
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