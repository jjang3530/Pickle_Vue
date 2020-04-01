<template>
  <tr>
    <td>{{ client.clientId }}</td>
    <td>{{ client.firstName }} {{ client.lastName }}</td>
    <td>{{ client.email }}</td>
    <td>
      <button class="mini ui blue button" @click="onEdit">Edit</button>
      <button class="mini ui red button" @click="onDelete">Delete</button>
    </td>
  </tr>
</template>

<script>
export default {
  name: "Customer",
  props: {
    client: {
      type: Object
    }
  },
  methods: {
    onDelete() {
      //console.log(this.client.clientId);
      this.$emit("onDelete", this.client.clientId);
    },
    onEdit() {
      console.log(this.client);
      this.$emit("onEdit", this.client);
    }
  }
};
</script>

<style scoped></style>
