<template>
  <div class="container-sm p-3">
    <table class="table table-striped">
      <thead>
        <tr>
          <th>ID</th>
          <th>Name</th>
          <th>Surname</th>
          <th>Address</th>
          <th>Action</th>
        </tr>
      </thead>

      <tbody>
        <tr v-for="stu of students" :key="stu.id">

          <!-- ID -->
          <td v-if="editmode === stu.id">
            <input type="text" class="form-control" v-model="stu.id" />
          </td>
          <td v-else>{{stu.id}}</td>

          <!-- Name -->
          <td v-if="editmode === stu.id">
            <input type="text" class="form-control" v-model="stu.name" />
          </td>
          <td v-else>{{stu.name}}</td>

          <!-- Surname -->
          <td v-if="editmode === stu.id">
            <input type="text" class="form-control" v-model="stu.surname" />
          </td>
          <td v-else>{{stu.surname}}</td>

          <!-- Address -->
          <td v-if="editmode === stu.id">
            <input type="text" class="form-control" v-model="stu.address1" />
            <input type="text" class="form-control" v-model="stu.district" />
            <input type="text" class="form-control" v-model="stu.province" />
            <input type="text" class="form-control" v-model="stu.postalCode" />
          </td>
          <td v-else>{{stu.address1}} {{stu.district}} {{stu.province}} {{stu.postalCode}}</td>
          
          <!-- Action -->
          <td class="text-right" v-if="editmode === stu.id">
            <button class="btn btn-success" @click="editstu(stu)">Save</button>&nbsp;
            <button class="btn btn-secondary" @click="chmode(stu, null)">Cancel</button>
          </td>

          <td class="text-right" v-else>
            <button class="btn btn-warning" @click="chmode(stu, stu.id)">Edit</button>&nbsp;
            <button class="btn btn-danger" @click="del(stu.id)">Delete</button>
          </td>

        </tr>
      </tbody>
    </table>
  </div>

  <div>
    <b-container class="bv-example-row bv-example-row-flex-cols">
      <b-row class="text-right">
        <b-col align-self="end">
          <button class="btn btn-warning" @click="chmode(stu, stu.id)">Add New</button>&nbsp;
        </b-col>
      </b-row>
    </b-container>
  </div>
  
</template>

<script>
export default {
  name: "StudentTable",
  props: {
    students: Array
  },
  data() {
    return {
      editmode: null
    }
  },
  methods: {
    chmode(stu, id) {
      if (id != null) {
        if (id != this.editmode && this.editmode !== null) {
          let found = this.students.find(cm => cm.id === this.editmode);
          Object.assign(found, this.customer);
        }
        this.editmode = stu.id;
        this.customer = Object.assign({}, stu)
      } else {
        this.editmode = null;
        Object.assign(stu, this.customer)

      }
    },
    editstu(stu) {
      if (stu.id === "" ||stu.name === "" || stu.surname === "" || stu.address1 === "" || stu.district === "" || stu.province === "" || stu.postalCode === "") {
        alert("All fields are required!");
        return;
      }
      this.$emit("editstu", stu.id, stu);
      this.editmode = null;
    }, 
    del(id) {
      this.$emit("delstu", id);
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>