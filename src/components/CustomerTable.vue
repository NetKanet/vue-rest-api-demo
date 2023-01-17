<template>
  <div class="container-sm p-3">
    <table class="table table-striped">
      <thead>
        <tr>
          <th>Customer Name</th>
          <th>Customer Tel</th>
          <th>Action</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="cus of customers" :key="cus.id">
          <td v-if="editmode === cus.id">
            <input type="text" class="form-control" v-model="cus.cusname" />
          </td>
          <td v-else>{{cus.cusname}}</td>
          <td v-if="editmode === cus.id">
            <input type="text" class="form-control" v-model="cus.custel" />
          </td>
          <td v-else>{{cus.custel}}</td>
          <td class="text-right" v-if="editmode === cus.id">
            <button class="btn btn-success" @click="editcus(cus)">Save</button>&nbsp;
            <button class="btn btn-secondary" @click="chmode(cus, null)">Cancel</button>
          </td>
          <td class="text-right" v-else>
            <button class="btn btn-warning" @click="chmode(cus, cus.id)">Edit</button>&nbsp;
            <button class="btn btn-danger" @click="del(cus.id)">Delete</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: "CustomerTable",
  props: {
    customers: Array
  },
  data() {
    return {
      editmode: null
    }
  },
  methods: {
    chmode(cus, id) {
      if (id != null) {
        if (id != this.editmode && this.editmode !== null) {
          let found = this.customers.find(cm => cm.id === this.editmode);
          Object.assign(found, this.customer);
        }
        this.editmode = cus.id;
        this.customer = Object.assign({}, cus)
      } else {
        this.editmode = null;
        Object.assign(cus, this.customer)

      }
    },
    editcus(cus) {
      if (cus.cusname === "" || cus.custel === "") {
        alert("All fields are required!");
        return;
      }
      this.$emit("editcus", cus.id, cus);
      this.editmode = null;
    }, 
    del(id) {
      this.$emit("delcus", id);
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>