<template>
    <b-card header="Contacts List">
      <b-container>
        <b-row>
          <b-col>
            <ManageContact  @addContact="addContact" md="2"/>
          </b-col>
          <b-col>
            <b-dropdown id="dropdown-dropright" dropright text="Sort" variant="primary" class="m-2">
              <b-dropdown-item @click="sortBy('firstName')" href="#">First Name</b-dropdown-item>
              <b-dropdown-item @click="sortBy('lastName')" href="#">Last Name</b-dropdown-item>
              <b-dropdown-item @click="sortBy('email')" href="#">Email</b-dropdown-item>
              <b-dropdown-item @click="sortBy('address.city')" href="#">City</b-dropdown-item>
              <b-dropdown-item @click="sortBy('address.zip')" href="#">Zip</b-dropdown-item>
            </b-dropdown>
          </b-col>
        </b-row>
      </b-container>
      <b-card-group deck>
        <b-card style="min-width: 50%;" class="m-3"
          :border-variant="contact.contact.frequencyId > 0 ? 'primary':'danger'"
          align="center"
          v-for="contact in contacts" :key="contact.id">
          <b-card-text><strong>Name:</strong>  {{contact.firstName}} {{contact.lastName}}</b-card-text>
          <b-card-text><strong>Email Address:</strong>  {{contact.email}}</b-card-text>
          <b-card-text><strong>City:</strong>  {{contact.address.city}}</b-card-text>
          <b-card-text><strong>Zip code:</strong>  {{contact.address.zip}}</b-card-text>
          <hr/>
          <b-row>
            <b-col>
              <b-button variant="danger" @click="deleteContact(contact.id)">Delete</b-button>
            </b-col>
            <b-col>
              <ManageContact :contact="contact" @updateContact="updateContact"/>
            </b-col>
          </b-row>
        </b-card>
      </b-card-group>
    </b-card>
</template>

<script>
import ManageContact from './ManageContact';
export default {
  props: [
    'contacts'
  ],
  components: {
    ManageContact
  },
  methods:{
    addContact(contact) {
      this.$emit('addContact', contact);
    },
    updateContact(payload) {
      console.log(payload);
      this.$emit('updateContact', payload);
    },
    deleteContact(contactId) {
      this.$emit('deleteContact', contactId);
    },
    sortBy(sortField) {
      console.log(sortField);
      this.$emit('sortBy', sortField);
    }
  }
}
</script>

<style>

</style>
