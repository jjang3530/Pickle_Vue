<template>
  <div id="app">
    <div class="ui fixed inverted menu vue-color">
      <div class="ui container">
        <a href="#" class="header item">Pickle</a>
      </div>
    </div>

    <div class="ui main container">
      <MyForm :form="form" @onFormSubmit="onFormSubmit" />
      <Loader v-if="loader" />
      <CustomerList
        :clients="clients"
        @onDelete="onDelete"
        @onEdit="onEdit"
      />
    </div>
  </div>
</template>

<script>
import axios from "axios";
import MyForm from "./components/MyForm";
import CustomerList from "./components/CustomerList";
import Loader from "./components/Loader";

export default {
  name: "App",
  components: {
    MyForm,
    CustomerList,
    Loader
  },
  data() {
    return {
      url: "https://mystic-centaur-271005.appspot.com/api/clients",
      clients: [],
      form: { firstName: "", lastName: "", email: "", isEdit: false },
      loader: false
    };
  },
  methods: {
    
    getCustomers() {
      this.loader = true;

      axios.get(this.url).then(data => {
        this.clients = data.data;
        this.loader = false;
      });
    },
    deleteCustomer(id) {
      this.loader = true;
      //console.log(id);

      axios
        .delete(`${this.url}/${id}`)
        .then(() => {
          this.getCustomers();
        })
        .catch(e => {
          alert(e);
        });
    },
    createCustomer(data) {
      this.loader = true;

      axios
        .post(this.url, {
          firstName: data.firstName,
          lastName: data.lastName,
          email: data.email
        })
        .then(() => {
          this.getCustomers();
        })
        .catch(e => {
          alert(e);
        });
    },
    editCustomer(data) {
      this.loader = true;
      //console.log(`${this.url}/${data.clientId}`);
      //console.log(data);

      axios
        .put(`${this.url}/${data.clientId}`, 
        
        {
          clientId: data.clientId,
          firstName: data.firstName,
          lastName: data.lastName,
          email: data.email
        },
        {
            headers: {
              'Accept': 'application/json',
              'Content-Type': 'application/json'
            }
         })
        .then((res) => {
          this.getCustomers();
          console.log(res.data)
        })
        .catch(error => {
    console.log(error.response)
});
    },
    onDelete(id) {
      // window.console.log("app delete " + id);

      this.deleteCustomer(id);
    },
    onEdit(data) {
      // window.console.log("app edit ", data);

      this.form = data;
      this.form.isEdit = true;
      
    },
    onFormSubmit(data) {
      // window.console.log("app onFormSubmit", data);

      if (data.isEdit) {
        // call edit client
        this.editCustomer(data);
      } else {
        // call create client
        this.createCustomer(data);
      }
    }
  },
  created() {
    this.getCustomers();
  }
};
</script>

<style>
.vue-color {
  background: #41b883 !important;
}

.main.container {
  margin-top: 60px;
}

.submit-button {
  margin-top: 24px !important;
  float: right;
}

.data {
  margin-top: 15px;
}

thead tr th {
  background: #e0e0e0 !important;
}

.ui.inverted.dimmer {
  background-color: rgba(255, 255, 255, 0) !important;
}
</style>
