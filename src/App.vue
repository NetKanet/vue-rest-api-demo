<template>
  <div id="app">
    <div class="container-sm p-3">
      <customer-form @addstu="addStudent" />
      <customer-table 
        :students="this.students"
        @editstu="editStudent"
        @delstu="delStudent"
         />
    </div>
  </div>
</template>

<script>
import CustomerForm from "./components/CustomerForm.vue";
import CustomerTable from "./components/CustomerTable.vue";

export default {
  name: "App",
  components: {
    CustomerForm,
    CustomerTable
  },
  data() {
    return {
      students: []
    };
  },
  methods: {
    async addStudent(student) {
      try {
        const response = await fetch("http://localhost:8080/v1/student/register",
          {
            method: "POST",
            // how to xml 
            body: JSON.stringify(student),
            headers: { "Content-type": "application/xml; charset=UTF-8" }
          }
        );
        const data = await response.json();
        this.students = [...this.students, data];
      } catch (error) {
        console.error(error);
      }
    },
    async delStudent(id) {
      try {
        await fetch("http://localhost:8080/v1/student/student-user/" + id, 
        {
          method: "DELETE"
        });
        this.students = this.students.filter(student => student.id !== id);
      } catch (error) {
        console.error(error);
      }
    },
    async editStudent(id, newcus) {
      try {
        const response = await fetch("http://localhost:8080/v1/student/" + id, 
        {
          method: "PUT",
          body: JSON.stringify(newcus),
          headers: { "Content-type": "application/xml; charset=UTF-8" }
        });
        const dat = await response.json();
        this.students = this.students.map(student =>
          student.id === id ? dat : student
        );
      } catch (error) {
        console.error(error);
      }
    },
    async getStudents() {
      try {
        const response = await fetch("http://localhost:8080/v1/student/");
        const cusdata = await response.json();
        this.students = cusdata;
      } catch (error) {
        console.error(error);
      }
    },
  },
  mounted() {
    this.getStudents();
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 20px;
}
</style>
