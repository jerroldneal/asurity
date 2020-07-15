<template>
  <b-col md="8" class="m-2">
    <b-card bg-variant="light">
      <b-form autocomplete="off">
        <b-form-group
          class="mb-0">
              <b-form-group label="First Name:" label-cols-sm="3" label-for="first-name">
                  <b-form-input v-model="form.firstName" id="first-name" name="firstName" trim :maxlength="20"></b-form-input>
              </b-form-group>
              <b-form-group label="Last Name:" label-cols-sm="3" label-for="last-name">
                <b-form-input v-model="form.lastName" id="lastName" name="last-name" trim :maxlength="20"></b-form-input>
              </b-form-group>
              <b-form-group label="Email Address:" label-cols-sm="3" label-for="email-address">
                <ValidationProvider name="Email Address" rules="required|email" v-slot="{errors}">
                  <b-form-input v-model="form.emailAddress" id="email-address" name="emailAddress" trim type="email"></b-form-input>
                  <span>{{ errors[0] }}</span>
                </ValidationProvider>
              </b-form-group>
              <b-form-group label="Street:" label-cols-sm="3" label-for="street">
                <b-form-input v-model="form.street" id="street" name="street" trim :maxlength="100"></b-form-input>
              </b-form-group>
              <b-form-group label="City" label-cols-sm="3">
                <b-form-input v-model="form.city" id="city" name="city" trim :maxlength="50"></b-form-input>
              </b-form-group>
              <b-form-group label="State" label-cols-sm="3">
                <b-form-select v-model="form.StateSelected" :options="form.StateOptions"></b-form-select>
              </b-form-group>
              <b-form-group label="Zip" label-cols-sm="3">
                <b-form-input v-model="form.Zip" id="zip" name="zip" trim></b-form-input>
              </b-form-group>
              <b-form-group label="Phone Number" label-cols-sm="3">
                <b-form-input v-model="form.PhoneNumber" id="phoneNumber" name="phoneNumber" trim></b-form-input>
              </b-form-group>
              <b-form-group label="Contact Frequency" label-cols-sm="3">
                <b-form-select v-model="form.ContactFrequencySelected" :options="form.ContactFrequencyOptions"></b-form-select>
              </b-form-group>
              <b-form-group label="Preferred Contact Method" label-cols-sm="3">
                <b-form-select v-model="form.ContactMethodSelected" :options="form.ContactMethodOptions"></b-form-select>
              </b-form-group>
          </b-form-group>
      </b-form>
      <b-button block variant="primary" @click="addContact">Add Contact</b-button>
    </b-card>
  </b-col>
</template>

<script>
import { extend } from 'vee-validate';

extend('even', value => {
  return value % 2 === 0;
});

export default {
  data() {
    return {
      form: {
        FirstName: null,
        LastName: null,
        EmailAddress: null,
        Street: null,
        City: null,
        Zip: null,
        PhoneNumber: null,
        StateSelected: null,
        StateOptions: [
          { value: null, text: 'Please select a state' },
          { value: 'WA', text: "Washington" },
          { value: 'DC', text: "Washington D.C."}
        ],

        ContactFrequencySelected: null,
        ContactFrequencyOptions: [
          { value: null, text: 'Please select an option' },
          { value: 0, text: 'Contact only about account information' },
          { value: 1, text: 'OK to contact with marketing information' },
          { value: 2, text: 'OK to contact with third-party marketing information' },
        ],
        ContactMethodSelected: null,
        ContactMethodOptions: [
          { value: null, text: 'Please select an option' },
          { value: 0, text: 'Text' },
          { value: 1, text: 'Email' },
          { value: 2, text: 'Phone' },
        ]
      },
      submitted: false

    }
  },
  methods: {
    async addContact() {
      console.log(this.$validator);
      this.submitted = true;
      console.log("form",this.form);
      //let result = await this.$validator.validate();
      //console.log('result', result);
      const {firstName, lastName, emailAddress, street, city, StateSelected, Zip, PhoneNumber, ContactFrequencySelected, ContactMethodSelected} = this.form;
      let newContact =  {
        firstName,
        lastName,
        email: emailAddress,
        address: {
          street,
          city,
          state: StateSelected,
          zip: Zip
        },
        phone: PhoneNumber,
        contact: {
          frequencyId: ContactFrequencySelected,
          methodId: ContactMethodSelected
        }
      };
      console.log('newContact',newContact);
      this.$emit('addContact',newContact);
    }
  }

}
</script>

<style>

</style>
