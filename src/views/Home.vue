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
      <button v-on:click="contactsDestroy(contact)">Delete a contact</button>
      </p>
      <hr>
      <dialog id = "contact-details">
        <form method = "dialog">
       <p> {{ currentContact.id }}</p>
       <input type = text v-model="currentContact.first_name" placeholder="currentContact.first_name">  </p>
       <p> <input type = text v-model="currentContact.last_name" placeholder="currentContact.last_name"> </p>
        <p> <input type = text v-model="currentContact.email" placeholder="currentContact.email"> </p>
        <p> <input type = text v-model="currentContact.phone_number" placeholder="currentContact.phone_number"> </p>
        <p> <input type = text v-model="currentContact.image" placeholder="currentContact.image"> </p>
        <button>Close</button>
        <button v-on:click="contactsUpdate(contact)">update</button>
        </form>
      </dialog>
  </div>
         <div> 
        <h2> Create a new contact </h2>
        <input v-model = "newContactFirstName" type = "text" placeholder="first_name">
        <input v-model = "newContactLastName" type = "text" placeholder="last_name">
        <input v-model = " newContactEmail" type = "text" placeholder="email">
        <input v-model = " newContactPhoneNumber" type = "text" placeholder="phone number">
        <input v-model = "newContactImage" type = "text" placeholder="image">
        <button v-on:click="contactsCreate">create contact</button>
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
      newContactFirstName: "",
      newContactLastName: "",
      newContactEmail: "",
      newContactPhoneNumber: "",
      newContactImage: "",
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
    contactsCreate: function () {
      var params = {
        first_name: this.newContactFirstName,
        last_name: this.newContactLastName,
        phone_number: this.newContactPhoneNumber,
        email: this.newContactEmail,
        image: this.newContactImage,
      };

      axios.post("http://localhost:3000/api/contacts", params).then((response) => {
        console.log(response.data);
      });
    },

    contactsUpdate: function (theContact) {
      var params = {
        first_name: this.currentContact.first_name,
        last_name: this.currentContact.last_name,
        phone_number: this.currentContact.phone_number,
        email: this.currentContact.email,
        image: this.currentContact.image,
      };
      axios.patch("http://localhost:3000/api/contacts/" + theContact.id, params).then((response) => {
        console.log(response.data);
        console.log("contact updated");
      });
    },
    contactsDestroy: function (theContact) {
      axios.delete("http://localhost:3000/api/contacts/" + theContact.id).then((response) => {
        console.log(response.data);
      });
    },
  },
};
</script>