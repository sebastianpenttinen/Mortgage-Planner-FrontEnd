<template>
  <div id="app" class="small-container">
    <h1>Mortage Planner</h1>
    <mortage-form @add:mortage="addMortage" />
    <mortage-table
      :mortages="mortages"
      @delete:mortage="deleteMortage"
      @edit:mortage="editMortage"
    />
  </div>
</template>

<script>
import MortageTable from "./components/MortageTable.vue";
import MortageForm from "./components/MortageForm.vue";

export default {
  name: "App",
  components: {
    MortageTable,
    MortageForm
  },
  data() {
    return {
      mortages: []
    };
  },

  mounted() {
    this.getMortages();
  },

  methods: {
    async getMortages() {
      try {
        const response = await fetch("http://localhost:8080/mortages/");
        const data = await response.json();
        this.mortages = data;
      } catch (error) {
        console.error(error);
      }
    },

    async addMortage(mortage) {
      try {
        const response = await fetch("http://localhost:8080/mortages/", {
          method: "POST",
          body: JSON.stringify(mortage),
          headers: { "Content-type": "application/json; charset=UTF-8" }
        });
        const data = await response.json();
        this.mortages = [...this.mortages, data];
      } catch (error) {
        console.error(error);
      }
    },

    async editMortage(id, updatedMortage) {
      try {
        const response = await fetch(`http://localhost:8080/mortages/${id}`, {
          method: "PUT",
          body: JSON.stringify(updatedMortage),
          headers: { "Content-type": "application/json; charset=UTF-8" }
        });
        const data = await response.json();
        this.mortages = this.mortages.map(mortage =>
          mortage.id === id ? data : mortage
        );
      } catch (error) {
        console.error(error);
      }
    },

    async deleteMortage(id) {
      try {
        await fetch(`http://localhost:8080/mortages/${id}`, {
          method: "DELETE"
        });
        this.mortages = this.mortages.filter(mortage => mortage.id !== id);
      } catch (error) {
        console.error(error);
      }
    }
  }
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
