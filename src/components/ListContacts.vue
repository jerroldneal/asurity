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
      <div>
        <b-card no-body>
          <b-tabs pills card>
            <b-tab title="Contacts" active>
              <b-card-text>
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
              </b-card-text>
            </b-tab>
            <b-tab title="Contact List">
              <b-card-text>
                  <div class="overflow-auto">
                    <b-pagination
                      v-model="currentPage"
                      :total-rows="rows"
                      :per-page="perPage"
                      aria-controls="contacts-table"
                    ></b-pagination>

                    <p class="mt-3">Current Page: {{ currentPage }}</p>

                    <b-table
                      id="contacts-table" striped hover
                      :items="contactsLimited"
                      :per-page="perPage"
                      :current-page="currentPage"
                      small
                    ></b-table>
                  <!-- <b-table id="contacts-tablex" striped hover :items="contacts"></b-table> -->
                </div>
              </b-card-text>
            </b-tab>
          </b-tabs>
        </b-card>
      </div>
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
  data() {
      return {
        perPage: 4,
        currentPage: 1
      }
  },
  computed: {
    rows() {
      return this.contacts.length
    },
    contactsLimited() {
      let result = this.contacts.map(contact => {
        let address = `${contact.address.street} ${contact.address.city} ${contact.address.state} ${contact.address.zip}`;
        let item = {
          "firstName": contact.firstName,
          "lastName": contact.lastName,
          "email": contact.email,
          address
        }
        return item;
      });
      return result;
    }
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
