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
      mortages: [
        {
          id: 1,
          name: "Juha",
          amount: 1000,
          interest: 5,
          years: 2
        },
        {
          id: 2,
          name: "Karvinen",
          amount: 4356,
          interest: 1.27,
          years: 6
        },
        {
          id: 3,
          name: "Claes",
          amount: 1300.55,
          interest: 8.67,
          years: 2
        },
        {
          id: 4,
          name: "Andersson",
          amount: 2000,
          interest: 6,
          years: 4
        }
      ]
    };
  },
  methods: {
    addMortage(mortage) {
      const lastId =
        this.mortages.length > 0
          ? this.mortages[this.mortages.length - 1].id
          : 0;
      const id = lastId + 1;
      const newMortage = { ...mortage, id };

      this.mortages = [...this.mortages, newMortage];
    },
    deleteMortage(id) {
      this.mortages = this.mortages.filter(mortage => mortage.id !== id);
    },
    editMortage(id, updatedMortage) {
      this.mortages = this.mortages.map(mortage =>
        mortage.id === id ? updatedMortage : mortage
      );
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
