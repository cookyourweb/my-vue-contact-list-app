<template>
  <div
    id="app"
    class="small-container"
  >
    <h1>Contacts</h1>

    <contact-form @add:contact="addContact" />
    <contact-table
      :contacts="contacts"
      @delete:contact="deleteContact"
      @edit:contact="editContact"
    />
  </div>
</template>

<script>
import ContactTable from '@/components/ContactTable.vue'
import ContactForm from '@/components/ContactForm.vue'

export default {
  name: "app",
  components: {
    ContactTable,
    ContactForm,
  },
  data() {
    return {
      contacts: []
    }
  },

  mounted() {
    this.getContacts()
  },

  methods: {
    async getContacts() {
      try {
        const response = await fetch(' http://localhost:8000/contacts')
        const data = await response.json()
        this.contacts = data
      } catch (error) {
        console.error(error)
      }
    },

    /* async addContact(contact) {
      try {
        const response = await fetch('http://localhost:8000/contacts/${id}', {
          method: 'POST',
          body: JSON.stringify(contact),
          headers: { "Content-type": "application/json; charset=UTF-8",'Content-Length': data.length }
          
        })
        const contact = await response.json()
        this.contacts = [...this.contacts, contact]
      } catch (error) {
        console.error(error)
        console.log("no inserta")
      }
    }, */
       async addContact(contact) {
      try {
        const response = await fetch('http://localhost:8000/contacts', {
          method: 'POST',
          body: JSON.stringify(contact),
          headers: { "Content-type": "application/json; charset=UTF-8" }
        })
        const data = await response.json()
        this.contacts = [...this.contacts, data]
      } catch (error) {
        console.error(error)
      }
    },

    async editContact(id, updatedContact) {
      try {
        const response = await fetch(`http://localhost:8000/contacts/${id}`, {
          method: 'PUT',
          body: JSON.stringify(updatedContact),
          headers: { "Content-type": "application/json; charset=UTF-8" }
        })
        const data = await response.json()
        this.contacts = this.contacts.map(contact => contact.id === id ? data : contact)
      } catch (error) {
        console.error(error)
        console.log("no modifica")
      }
    },

    async deleteContact(id) {
      try {
        await fetch(`http://localhost:8000/contacts/${id}`, {
          method: 'DELETE'
        })
        this.contacts = this.contacts.filter(contact => contact.id !== id)
      } catch (error) {
        console.error(error)
      }
    },
  },
}
</script>

<style>
button {
  background: #009435;
  border: 1px solid #009435;
}

button:hover,
button:active,
button:focus {
  background: #32a95d;
  border: 1px solid #32a95d;
}

.small-container {
  max-width: 680px;
}
</style>
