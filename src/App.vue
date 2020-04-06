<template>
  <div id="app" class="small-container">
    <h1>Mortgage Planner</h1>
    <mortgage-form @add:mortgage="addMortgage" />
    <mortgage-table
      :mortgages="mortgages"
      @delete:mortgage="deleteMortgage"
      @edit:mortgage="editMortgage"
    />
  </div>
</template>

<script>
import MortgageTable from "./components/MortgageTable.vue";
import MortgageForm from "./components/MortgageForm.vue";

export default {
  name: "App",
  components: {
    MortgageTable,
    MortgageForm,
  },
  data() {
    return {
      mortgages: [],
    };
  },

  mounted() {
    this.getMortgages();
  },

  methods: {
    async getMortgages() {
      try {
        const response = await fetch("http://localhost:8080/mortgages/");
        const data = await response.json();
        this.mortgages = data;
      } catch (error) {
        console.error(error);
      }
    },

    async addMortgage(mortgage) {
      try {
        const response = await fetch("http://localhost:8080/mortgages/", {
          method: "POST",
          body: JSON.stringify(mortgage),
          headers: { "Content-type": "application/json; charset=UTF-8" },
        });
        const data = await response.json();
        this.mortgages = [...this.mortgages, data];
      } catch (error) {
        console.error(error);
      }
    },

    async editMortgage(id, updatedMortgage) {
      try {
        const response = await fetch(`http://localhost:8080/mortgages/${id}`, {
          method: "PUT",
          body: JSON.stringify(updatedMortgage),
          headers: { "Content-type": "application/json; charset=UTF-8" },
        });
        const data = await response.json();
        this.mortgages = this.mortgages.map((mortgage) =>
          mortgage.id === id ? data : mortgage
        );
      } catch (error) {
        console.error(error);
      }
    },

    async deleteMortgage(id) {
      try {
        await fetch(`http://localhost:8080/mortgages/${id}`, {
          method: "DELETE",
        });
        this.mortgages = this.mortgages.filter(
          (mortgage) => mortgage.id !== id
        );
      } catch (error) {
        console.error(error);
      }
    },
  },
};
</script>

<style>
button {
  background: #009435;
  border: 1px solid #009435;
}

.small-container {
  max-width: 680px;
}
</style>
