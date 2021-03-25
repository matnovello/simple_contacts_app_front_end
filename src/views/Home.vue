<template>
  <div class="app">
    <h1>{{ message }}</h1>
    <button v-on:click="contactsIndex">toggle index</button>
    <div v-for= "contact in contacts" v-if="toggleContacts"> 
      <p> {{ contact.id }} 
      {{ contact.first_name }}
      {{ contact.last_name }}
      {{ contact.email }}
      {{ contact.phone_number }}
      {{ contact.image}}
      </p>

    </div>
  </div>
</template>

<style>
</style>

<script>
import axios from "axios";

export default {
  data: function () {
    return {
      message: "Welcome to Vue.js!",
      test: "testing",
      contacts: "",
      toggleContacts: false,
    };
  },
  created: function () {},
  methods: {
    contactsIndex: function () {
      axios.get("http://localhost:3000/api/contacts").then((response) => {
        console.log(response.data);
        this.contacts = response.data;
        this.toggleContacts = !this.toggleContacts;
      });
    },
  },
};
</script>