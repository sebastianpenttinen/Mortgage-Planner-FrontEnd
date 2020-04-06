<template>
  <div id="mortgage-form">
    <form @submit.prevent="handleSubmit">
      <label>Name</label>
      <input
        ref="first"
        type="text"
        :class="{ 'has-error': submitting && invalidName }"
        v-model="mortgage.name"
        @focus="clearStatus"
        @keypress="clearStatus"
      />
      <label>Amount to borrow</label>
      <input
        type="number"
        :class="{ 'has-error': submitting && invalidAmount }"
        v-model="mortgage.amount"
        @focus="clearStatus"
      />
      <label>Interest</label>
      <input
        type="number"
        :class="{ 'has-error': submitting && invalidInterest }"
        v-model="mortgage.interest"
        @focus="clearStatus"
      />
      <label>Years</label>
      <input
        type="number"
        :class="{ 'has-error': submitting && invalidYears }"
        v-model="mortgage.years"
        @focus="clearStatus"
      />
      <p v-if="error && submitting" class="error-message">❗Please fill out all required fields</p>
      <p v-if="success" class="success-message">✅ Employee successfully added</p>
      <button>Calculate mortgage</button>
    </form>
  </div>
</template>

<script>
export default {
  name: "mortgage-form",
  data() {
    return {
      submitting: false,
      error: false,
      success: false,
      mortgage: {
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

      this.$emit("add:mortgage", this.mortgage);
      this.$refs.first.focus();
      this.mortgage = {
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
      return this.mortgage.name === "";
    },

    invalidAmount() {
      return this.mortgage.amount === "";
    },
    invalidInterest() {
      return this.mortgage.interest === "";
    },
    invalidYears() {
      return this.mortgage.years === "";
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