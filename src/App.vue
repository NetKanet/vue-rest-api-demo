<template>
  <div id="app">
    <div class="container-sm p-3">
      <customer-form @addcus="addCustomer" />
      <hr />
      <h2>Customers List</h2>
      <hr />
      <customer-table 
        :customers="this.customers"
        @editcus="editCustomer"
        @delcus="delCustomer"
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
      customers: []
    };
  },
  methods: {
    async addCustomer(customer) {
      try {
        const response = await fetch("http://192.168.1.125:9000/cus",
          {
            method: "POST",
            body: JSON.stringify(customer),
            headers: { "Content-type": "application/json; charset=UTF-8" }
          }
        );
        const data = await response.json();
        this.customers = [...this.customers, data];
      } catch (error) {
        console.error(error);
      }
    },
    async delCustomer(id) {
      try {
        await fetch("http://localhost:9000/cus/" + id, 
        {
          method: "DELETE"
        });
        this.customers = this.customers.filter(customer => customer.id !== id);
      } catch (error) {
        console.error(error);
      }
    },
    async editCustomer(id, newcus) {
      try {
        const response = await fetch("http://localhost:9000/cus/" + id, 
        {
          method: "PUT",
          body: JSON.stringify(newcus),
          headers: { "Content-type": "application/json; charset=UTF-8" }
        });
        const dat = await response.json();
        this.customers = this.customers.map(customer =>
          customer.id === id ? dat : customer
        );
      } catch (error) {
        console.error(error);
      }
    },
    async getCustomers() {
      try {
        const response = await fetch("http://localhost:9000/cus");
        const cusdata = await response.json();
        this.customers = cusdata;
      } catch (error) {
        console.error(error);
      }
    },
  },
  mounted() {
    this.getCustomers();
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
