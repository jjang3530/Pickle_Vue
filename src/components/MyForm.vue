<template>
  <div class="my-form">
    <form class="ui form">
      <div class="fields">
        <div class="four wide field">
          <label>First Name</label>
          <input
            ref="firstName"
            type="text"
            name="firstName"
            placeholder="First Name"
            @change="handleChange"
            :value="form.firstName"
          />
        </div>

        <div class="four wide field">
          <label>Last Name</label>
          <input
            type="text"
            name="lastName"
            placeholder="Last Name"
            @change="handleChange"
            :value="form.lastName"
          />
        </div>

        <div class="six wide field">
          <label>E-mail</label>
          <input
            type="email"
            name="email"
            placeholder="dima@gmail.com"
            @change="handleChange"
            :value="form.email"
          />
        </div>

        <div class="two wide field">
          <button :class="btnClass" @click="onFormSubmit">
            {{ btnName }}
          </button>
        </div>
      </div>
    </form>
  </div>
</template>

<script>
export default {
  name: "MyForm",
  data() {
    return {
      btnName: "Save",
      btnClass: "ui primary button submit-button"
    };
  },
  props: {
    form: {
      type: Object
    }
  },
  methods: {
    // focusInput() {
    //   this.$refs.firstName.focus();
    // },
    handleChange(event) {
      const { name, value } = event.target;
      let form = this.form;
      form[name] = value;
      this.form = form;
    },
    onFormSubmit(event) {
      // prevent form submit
      event.preventDefault();

      // form validation
      if (this.formValidation()) {
        // window.console.log("ready to submit");
        this.$emit("onFormSubmit", this.form);
        
        //this.focusInput();

        // change the button to save
        this.btnName = "Save";
        this.btnClass = "ui primary button submit-button";

        // clear form fields
        //this.clearFormFields();
      }
    },
    formValidation() {
      // first name
      if (document.getElementsByName("firstName")[0].value === "") {
        alert("Enter first name");
        return false;
      }

      // last name
      // if (document.getElementsByName("lastName")[0].value === "") {
      //   alert("Enter last name");
      //   return false;
      // }

      // email
      // if (document.getElementsByName("email")[0].value === "") {
      //   alert("Enter email");
      //   return false;
      // }

      return true;
    },
    clearFormFields() {
      // clear form data
      this.form.firstName = "";
      this.form.lastName = "";
      this.form.email = "";
      this.form.isEdit = false;

      // clear form fields
      document.querySelector(".form").reset();
    }
  },
  updated() {
    if (this.form.isEdit) {
      this.btnName = "Update";
      this.btnClass = "ui orange button submit-button";
    }
  }
};
</script>

<style scoped></style>
