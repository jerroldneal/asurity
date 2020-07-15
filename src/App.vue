<template>
  <div id="app">
    <AppHeader />
    <b-container>
      <b-row class="justify-content-center">
        <ListContacts :contacts="contactList" @addContact="addContactHandler" @updateContact="updateContactHandler" @deleteContact="deleteContactHandler"/>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import AppHeader from './components/AppHeader.vue';
import ListContacts from './components/ListContacts.vue';
import axios from 'axios';

export default {
  name: 'App',
  components: {
    AppHeader,
    ListContacts
  },
  data() {
    return {
      contactList: []
    }
  },
  methods: {
    async getContactList() {
      let result = await axios.get("http://localhost:3000/contacts");
      this.contactList = result.data;
    },
    async addContactHandler(payload) {
      try {
          await axios.post('http://localhost:3000/contacts', payload);
          await this.getContactList();
      } catch(error) {
        console.log(error);
      }
    },
    async updateContactHandler(payload) {
      console.log(payload);
      try {
      let url = `http://localhost:3000/contacts/${payload.original.id}`;
          await axios.put(url, payload.updated);
          await this.getContactList();
      } catch(error) {
        console.log(error);
      }
    },
    async deleteContactHandler(contactId) {
      let url = `http://localhost:3000/contacts/${contactId}`;
      try {
          await axios.delete(url);
          await this.getContactList();
      } catch(error) {
        console.log(error);
      }
    }

  },
  mounted() {
    this.getContactList();
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.error-message {
  color:red;
}
</style>
