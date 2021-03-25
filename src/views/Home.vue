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
      <button v-on:click="contactsShow(contact)">click for more info</button>
      </p>
      <hr>
      <dialog id = "contact-details">
        <form method = "dialog">
       <p> {{ currentContact.id }} </p>
        <p> {{ currentContact.first_name }} </p>
        <p> {{ currentContact.last_name }} </p>
        <p> {{ currentContact.email }} </p>
        <p> {{ currentContact.phone_number }} </p>
        <p> {{ currentContact.image }} </p>
        <button>Close</button>
        </form>
      </dialog>
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
      currentContact: "",
    };
  },
  created: function () {
    this.contactsIndex();
  },
  methods: {
    contactsIndex: function () {
      axios.get("http://localhost:3000/api/contacts").then((response) => {
        console.log(response.data);
        this.contacts = response.data;
        this.toggleContacts = !this.toggleContacts;
      });
    },
    contactsShow: function (theContact) {
      this.currentContact = theContact;
      console.log(theContact);
      document.querySelector("#contact-details").showModal();
    },
  },
};
</script>