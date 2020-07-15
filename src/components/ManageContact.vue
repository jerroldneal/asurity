<template>
  <div>
    <b-button @click="onEditClicked">{{this.$props.contact == null ? "Add" : "Edit"}}</b-button>
    <b-modal v-model="isVisible" @show="onShow" @hidden="onHidden" @ok="onOkay" :title="this.$props.contact == null ? 'New Contact' : 'Update Contact'">
    <b-form autocomplete="off">
        <b-form-group
          class="mb-0">
              <b-form-group label="First Name:" label-cols-sm="3" label-for="first-name">
                  <b-form-input v-validate="{required: true, max:20}" v-model="form.firstName" id="first-name" name="firstName" trim></b-form-input>
                  <div v-if="submitted" class="error-message">
                    {{errors.first('firstName')}}
                  </div>
              </b-form-group>
              <b-form-group label="Last Name:" label-cols-sm="3" label-for="last-name">
                <b-form-input v-validate="{required: true, max:20}" v-model="form.lastName" id="lastName" name="last-name" trim :maxlength="20"></b-form-input>
                  <div v-if="submitted" class="error-message">
                  {{errors.first('last-name')}}
                  </div>
              </b-form-group>
              <b-form-group label="Email Address:" label-cols-sm="3" label-for="email-address">
                <b-form-input v-validate="'required|email'" v-model="form.emailAddress" id="email-address" name="emailAddress" trim type="email"></b-form-input>
                  <div v-if="submitted" class="error-message">
                  {{errors.first('emailAddress')}}
                  </div>
              </b-form-group>
              <b-form-group label="Street:" label-cols-sm="3" label-for="street">
                <b-form-input v-validate="{required: true, max:100}" v-model="form.street" id="street" name="street" trim :maxlength="100"></b-form-input>
                  <div v-if="submitted" class="error-message">
                  {{errors.first('street')}}
                  </div>
              </b-form-group>
              <b-form-group label="City" label-cols-sm="3">
                <b-form-input v-validate="{required: true, max:50}" v-model="form.city" id="city" name="city" trim :maxlength="50"></b-form-input>
                  <div v-if="submitted" class="error-message">
                  {{errors.first('city')}}
                  </div>
              </b-form-group>
              <b-form-group label="State" label-cols-sm="3">
                <b-form-select v-validate="{required: true}" name="state" v-model="form.StateSelected" :options="form.StateOptions"></b-form-select>
                  <div v-if="submitted" class="error-message">
                  {{errors.first('state')}}
                  </div>
              </b-form-group>
              <b-form-group label="Zip" label-cols-sm="3">
                <b-form-input v-validate="{required: true}" v-model="form.Zip" id="zip" name="zip" trim></b-form-input>
                  <div v-if="submitted" class="error-message">
                  {{errors.first('zip')}}
                  </div>
              </b-form-group>
              <b-form-group label="Phone Number" label-cols-sm="3">
                <b-form-input v-validate="{required: true}" v-model="form.PhoneNumber" id="phoneNumber" name="phoneNumber" trim></b-form-input>
                  <div v-if="submitted" class="error-message">
                  {{errors.first('phoneNumber')}}
                  </div>
              </b-form-group>
              <b-form-group label="Contact Frequency" label-cols-sm="3">
                <b-form-select v-validate="{required: true}" name="frequency" v-model="form.ContactFrequencySelected" :options="form.ContactFrequencyOptions"></b-form-select>
                  <div v-if="submitted" class="error-message">
                  {{errors.first('frequency')}}
                  </div>
              </b-form-group>
              <b-form-group label="Preferred Contact Method" label-cols-sm="3">
                <b-form-select v-validate="{required: true}" name="method" v-model="form.ContactMethodSelected" :options="form.ContactMethodOptions"></b-form-select>
                  <div v-if="submitted" class="error-message">
                  {{errors.first('method')}}
                  </div>
              </b-form-group>
          </b-form-group>
      </b-form>
    </b-modal>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  name: 'ManageContact',
  props: [
    'contact'
  ],
  data() {
    return {
      isVisible: false,
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
    onEditClicked() {
      this.isVisible = true;
    },
    onShow(e) {
      console.log(e);
      this.loadForm(this.$props.contact);
    },
    onHidden(e) {
      console.log(e);
      this.form = {};
    },
    async onOkay(e) {
      console.log(this.$validator);
      console.log(e);
      e.preventDefault();
      this.submitted = true;
      let result = await this.$validator.validate();
      console.log(result);
      if(result === true) {
        let formData = this.retrieveFormData();
        console.log(formData);
        if(this.$props.contact == null) {
          this.$emit('addContact',formData);
        } else {
          let payload = {original:this.$props.contact,updated:formData};
          this.$emit('updateContact', payload);
        }

        this.isVisible = false;
        this.submitted = false;
      }
    },
    initForm() {
      this.form = {
        firstName: null,
        lastName: null,
        emailAddress: null,
        PhoneNumber: null,
        street: null,
        city: null,
        Zip: null,
        StateSelected: null,
        ContactFrequencySelected: null,
        ContactMethodSelected: null,
        ContactFrequencyOptions: [
          { value: null, text: 'Please select an option' },
        ],
        ContactMethodOptions: [
          { value: null, text: 'Please select an option' },
        ],
        StateOptions: [
          { value: null, text: 'Please select a state' },
          { value: 'DC', text: "Washington D.C."}
        ]
      }
    },
    async loadForm(contact) {     
      await this.loadOptionsData();      
      if(contact == null) {
        this.initForm();
      } else {
        const {firstName, lastName, email, phone} = this.$props.contact;
        const {street, city, state, zip} = this.$props.contact.address;
        const {frequencyId, methodId} = this.$props.contact.contact;
        this.form = {
          firstName,
          lastName,
          emailAddress: email,
          PhoneNumber: phone,
          street,
          city,
          Zip: zip,
          StateSelected: state,
          ContactFrequencySelected: frequencyId,
          ContactMethodSelected: methodId,
          ContactFrequencyOptions: [
            { value: null, text: 'Please select an option' },
          ],
          ContactMethodOptions: [
            { value: null, text: 'Please select an option' },
          ],
          StateOptions: [
            { value: null, text: 'Please select a state' },
          ]
        };
      }
      await this.initializeStateOptions();
      await this.initializeContactFrequencyOptions();
      await this.initializeContactMethodOptions();
    },
    async initializeStateOptions() {
      let selected = this.form.StateSelected;
      let stateOptions = await this.getList("states");
      let formStateOptions = stateOptions.map(state => { return ({value: state.abbreviation, text: state.name}) });
      this.form.StateOptions = this.form.StateOptions.concat(formStateOptions);
      this.form.StateSelected = selected;
    },
    async initializeContactFrequencyOptions() {
      let selected = this.form.ContactFrequencySelected;
      let contactFrequencyOptions = await this.getList("contactFrequencies");
      let formContactFrequencyOptions = contactFrequencyOptions.map(cfo => { return ({value: cfo.id, text: cfo.description}) });
      this.form.ContactFrequencyOptions = this.form.ContactFrequencyOptions.concat(formContactFrequencyOptions);
      this.form.ContactFrequencySelected = selected;
    },
    async initializeContactMethodOptions() {
      let selected = this.form.ContactMethodSelected;
      let contactMethodOptions = await this.getList("contactMethods");
      let formContactMethodOptions = contactMethodOptions.map(cfo => { return ({value: cfo.id, text: cfo.description}) });
      this.form.ContactMethodOptions = this.form.ContactMethodOptions.concat(formContactMethodOptions);
      this.form.ContactMethodSelected = selected;
    },
    retrieveFormData() {
      const {firstName, lastName, emailAddress, street, city, StateSelected, Zip, PhoneNumber, ContactFrequencySelected, ContactMethodSelected} = this.form;
      let formData =  {
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
      return formData;
    },
    async loadOptionsData() {
      let stateOptions = await this.getList("states");
      console.log(stateOptions);
      let formStateOptions = stateOptions.map(state => { return ({value: state.abbreviation, text: state.name}) });
      console.log("formStateOptions",formStateOptions);
      this.form.stateOptions = formStateOptions;
    },
    async getList(collectionName) {
      let url = `http://localhost:3000/${collectionName}`;
      let result = await axios.get(url);
      console.log('getList', collectionName, result, result.data);
      return result.data;
    }
  }
}
</script>

<style>

</style>
