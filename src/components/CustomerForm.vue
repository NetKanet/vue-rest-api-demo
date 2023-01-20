<template>
  <div class="text-left">
    <form @submit.prevent="stusubmit">
      <div class="form-group">
        <label>ID:</label>
        <input type="text" class="form-control" v-model="student.id" required />
      </div>

      <div class="form-group">
        <label>Name and Surname:</label>
        <input type="text" class="form-control" v-model="student.name" required />
      </div>

      <div class="form-group">
        <label>Address:</label>
        <input type="text" class="form-control" v-model="student.address1" required />
      </div>

      <button type="submit" class="btn btn-primary">SAVE</button>
    </form>
  </div>
</template>

<script>
export default {
  name: "CustomerForm",
  data() {
    return {
      student: {
        id: "",
        name: "",
        address1: ""
      }
    };
  },
  methods: {
    stusubmit() {
      this.$emit("addstu", this.customer);
      this.student.id = "";
      this.student.name = "";
      this.student.address1 = "";
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
