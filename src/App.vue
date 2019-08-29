<template>
  <div id="app" class="small-container">
    <h1>Employees</h1>
    <employee-form @add:employee="addEmployee" />
    <employee-table v-bind:employees="employees" @delete:employee="deleteEmployee" @edit:employee="editEmployee" />
  </div>
</template>

<script>
import EmployeeTable from '@/components/EmployeeTable.vue'
import EmployeeForm from '@/components/EmployeeForm.vue'

export default {
  name: 'app',
  components: {
    EmployeeTable,
    EmployeeForm
  },
  data() {
    return {
      employees: [],
    } 
  },
  mounted() {
    this.getEmployees()
  },
  methods: {
    async getEmployees() {
      try {
        const response = await fetch('https://jsonplaceholder.typicode.com/users')
        const data = await response.json()
        this.employees = data
      } catch (error) {
        console.log(error)
      }
    },
    async addEmployee(employee) {
      try {
        const response = await fetch('https://jsonplaceholder.typicode.com/users', {
          method: 'POST',
          body: JSON.stringify(employee),
          headers: {'content-type': 'application/json; charset=UTF-8'}
        })
        const data = await response.json()
        this.employees = [...employees, data]
      } catch (error) {
        console.log(error)
      }
    },
    async editEmployee(id, updatedEmployee) {
      try {
        const response = await fetch('https://jsonplaceholder.typicode.com/users/${id}', {
          method: 'PUT',
          body: JSON.stringify(updatedEmployee),
          headers: {'content-type': 'application/json; charset=UTF-8'}
        })
        const data = await response.json()
        this.employees = this.employees.map(employee => employee.id === id ? data: employee)
      } catch(error) {
        console.log(error)
      }
    },
    async deleteEmployee(id) {
      try {
        const response = await fetch('https://jsonplaceholder.typicode.com/users/${id}', {
          method: 'DELETE'
        });
        this.employees = this.employees.filter(employee => employee.id !== id);
      } catch (error) {
        console.log(error)
      }
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
