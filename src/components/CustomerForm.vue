<template>
  <div class="text-left">
    <form @submit.prevent="cussubmit">
      <div class="form-group">
        <label>Customer Name :</label>
        <input type="text" class="form-control" v-model="customer.cusname" required />
      </div>
      <div class="form-group">
        <label>Customer tel :</label>
        <input type="text" class="form-control" v-model="customer.custel" required />
      </div>
      <button type="submit" class="btn btn-primary">Add Customer</button>
    </form>
  </div>
</template>

<script>
export default {
  name: "CustomerForm",
  data() {
    return {
      customer: {
        cusname: "",
        custel: ""
      }
    };
  },
  methods: {
    cussubmit() {
      this.$emit("addcus", this.customer);
      this.customer.cusname = "";
      this.customer.custel = "";
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
