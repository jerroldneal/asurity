<template>
    <b-col md="4" class="m-3">
    <b-card header="Contacts List">
      <ManageContact  @addContact="addContact"/>
      <b-card-group deck>
        <b-card style="min-width: 50%" class="m-3"
          :border-variant="contact.contact.frequencyId > 0 ? 'primary':'danger'"
          align="center"
          v-for="contact in contacts" :key="contact.id">
          <b-card-text><strong>Name:</strong>  {{contact.firstName}} {{contact.lastName}}</b-card-text>
          <b-card-text><strong>Email Address:</strong>  {{contact.email}}</b-card-text>
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
  </b-col>
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
    }
  }
}
</script>

<style>

</style>
