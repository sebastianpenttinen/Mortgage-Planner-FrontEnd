<template>
  <div id="mortage-table">
    <p v-if="mortages.length < 1" class="empty-table">No Mortages</p>
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
        <tr v-for="mortage in mortages" :key="mortage.id">
          <td v-if="editing === mortage.id">
            <input type="text" v-model="mortage.name" />
          </td>
          <td v-else>{{mortage.name}}</td>

          <td v-if="editing === mortage.id">
            <input type="text" v-model="mortage.amount" />
          </td>
          <td v-else>{{mortage.amount}}</td>

          <td v-if="editing === mortage.id">
            <input type="text" v-model="mortage.years" />
          </td>
          <td v-else>{{mortage.years}}</td>

          <td v-if="editing === mortage.id">
            <input type="text" v-model="mortage.interest" />
          </td>
          <td v-else>{{mortage.interest}}</td>

          <td>{{mortage.monthlyPayment.toFixed(2)}}</td>

          <td v-if="editing === mortage.id">
            <button @click="editMortage(mortage)">Save</button>
            <button class="muted-button" @click="editing = null">Cancel</button>
          </td>
          <td v-else>
            <button @click="editMode(mortage.id)">Edit</button>
            <button @click="$emit('delete:mortage', mortage.id)">Delete</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: "mortage-table",
  props: {
    mortages: Array
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
    editMortage(mortage) {
      if (
        mortage.name === "" ||
        mortage.amount === "" ||
        mortage.years === "" ||
        mortage.interest === ""
      ) {
        return;
      }

      this.$emit("edit:mortage", mortage.id, mortage);
      this.editing = null;
    },
    cancelEdit(mortage) {
      Object.assign(mortage, this.cachedMortage);
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