<template>
  <div id="employee-table">
    <p v-if="employees.length < 1" class="empty-table">No employees</p>
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
        <tr v-for="employee in employees" :key="employee.id">
          <td v-if="editing === employee.id">
            <input type="text" v-model="employee.name" />
          </td>
          <td v-else>{{employee.name}}</td>

          <td v-if="editing === employee.id">
            <input type="text" v-model="employee.amount" />
          </td>
          <td v-else>{{employee.amount}}</td>

          <td v-if="editing === employee.id">
            <input type="text" v-model="employee.years" />
          </td>
          <td v-else>{{employee.years}}</td>

          <td v-if="editing === employee.id">
            <input type="text" v-model="employee.interest" />
          </td>
          <td v-else>{{employee.interest}}</td>

          <td v-if="editing === employee.id">
            <input type="text" v-model="employee.montly" />
          </td>
          <td v-else>{{employee.montly}}</td>

          <td v-if="editing === employee.id">
            <button @click="editEmployee(employee)">Save</button>
            <button class="muted-button" @click="editing = null">Cancel</button>
          </td>
          <td v-else>
            <button @click="editMode(employee.id)">Edit</button>
            <button @click="$emit('delete:employee', employee.id)">Delete</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: "employee-table",
  props: {
    employees: Array
  },
  data() {
    return {
      editing: null
    };
  },
  methods: {
    editMode(id) {
      this.editing = id;
    }
  },
  editEmployee(employee) {
    if (employee.name === "" || employee.email === "") return;
    this.$emit("edit:employee", employee.id, employee);
    this.editing = null;
  },
  cancelEdit(employee) {
    Object.assign(employee, this.cachedEmployee);
    this.editing = null;
  }
};
</script>
<style scoped>
button {
  margin: 0 0.5rem 0 0;
}
</style>