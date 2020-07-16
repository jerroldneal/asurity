<template>
  <div id="app">
    <AppHeader />
    <b-container>
      <b-row class="justify-content-center">
        <b-col>
          <ListContacts :contacts="contactList" @addContact="addContactHandler" @updateContact="updateContactHandler" @deleteContact="deleteContactHandler" @sortBy="sortByHandler"/>
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import { Component, Vue } from 'vue-property-decorator';
import AppHeader from './components/AppHeader.vue';
import ListContacts from './components/ListContacts.vue';
import axios from 'axios';


@Component({
  name: 'App',
  components: {
    AppHeader,
    ListContacts
  },
  data() {
    return {
      contactList: [],
      orderByList: ['email']
    }
  },
  methods: {
    async getContactList() {
      const data = await this.getList("contacts");
      this.contactList = data;
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
      const url = `http://localhost:3000/contacts/${payload.original.id}`;
          await axios.put(url, payload.updated);
          await this.getContactList();
      } catch(error) {
        console.log(error);
      }
    },
    async deleteContactHandler(contactId) {
      const url = `http://localhost:3000/contacts/${contactId}`;
      try {
          await axios.delete(url);
          await this.getContactList();
      } catch(error) {
        console.log(error);
      }
    },
    async sortByHandler(sortByField) {
      console.log(sortByField);
      this.orderByList = [sortByField];
      await this.getContactList();
    },
    async getList(collectionName) {
      const sortOrder = this.orderByList ? "?_sort="+this.orderByList.join("&") : "";
      const url = `http://localhost:3000/${collectionName}${sortOrder}`;
      const result = await axios.get(url);
      return result.data;
    },

  },
  mounted() {
    this.getContactList();
  }
})
export default class App extends Vue {}
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
