<template>
  <div id="app" class="small-container">
    <h1>Mortage Planner</h1>
    <employee-form @add:employee="addEmployee" />
    <employee-table
      :employees="employees"
      @delete:employee="deleteEmployee"
      @edit:employee="editEmployee"
    />
  </div>
</template>

<script>
import EmployeeTable from "./components/EmployeeTable.vue";
import EmployeeForm from "./components/EmployeeForm.vue";

export default {
  name: "App",
  components: {
    EmployeeTable,
    EmployeeForm
  },
  data() {
    return {
      employees: [
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
    addEmployee(employee) {
      const lastId =
        this.employees.length > 0
          ? this.employees[this.employees.length - 1].id
          : 0;
      const id = lastId + 1;
      const newEmployee = { ...employee, id };

      this.employees = [...this.employees, newEmployee];
    },
    deleteEmployee(id) {
      this.employees = this.employees.filter(employee => employee.id !== id);
    },
    editEmployee(id, updatedEmployee) {
      this.employees = this.employees.map(employee =>
        employee.id === id ? updatedEmployee : employee
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
