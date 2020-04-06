<template>
  <div id="mortgage-table">
    <p v-if="mortgages.length < 1" class="empty-table">No mortgages</p>
    <table>
      <thead>
        <tr>
          <th>Name</th>
          <th>Amount to borrow (€)</th>
          <th>Years</th>
          <th>Interest (%)</th>
          <th>Monthly payment (€)</th>
          <th>Actions</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="mortgage in mortgages" :key="mortgage.id">
          <td v-if="editing === mortgage.id">
            <input type="text" v-model="mortgage.name" />
          </td>
          <td v-else>{{mortgage.name}}</td>

          <td v-if="editing === mortgage.id">
            <input type="text" v-model="mortgage.amount" />
          </td>
          <td v-else>{{mortgage.amount}}</td>

          <td v-if="editing === mortgage.id">
            <input type="text" v-model="mortgage.years" />
          </td>
          <td v-else>{{mortgage.years}}</td>

          <td v-if="editing === mortgage.id">
            <input type="text" v-model="mortgage.interest" />
          </td>
          <td v-else>{{mortgage.interest}}</td>

          <td>{{mortgage.monthlyPayment.toFixed(2)}}</td>

          <td v-if="editing === mortgage.id">
            <button @click="editMortgage(mortgage)">Save</button>
            <button class="muted-button" @click="editing = null">Cancel</button>
          </td>
          <td v-else>
            <button @click="editMode(mortgage.id)">Edit</button>
            <button @click="$emit('delete:mortgage', mortgage.id)">Delete</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: "mortgage-table",
  props: {
    mortgages: Array
  },
  data() {
    return {
      editing: null
    };
  },
  methods: {
    editMode(id) {
      this.editing = id;
    },
    editMortgage(mortgage) {
      if (
        mortgage.name === "" ||
        mortgage.amount === "" ||
        mortgage.years === "" ||
        mortgage.interest === ""
      ) {
        return;
      }

      this.$emit("edit:mortgage", mortgage.id, mortgage);
      this.editing = null;
    },
    cancelEdit(mortgage) {
      Object.assign(mortgage, this.cachedmortgage);
      this.editing = null;
    }
  }
};
</script>
<style scoped>
button {
  margin: 0 0.5rem 0 0;
}
td {
  white-space: nowrap;
}
</style>