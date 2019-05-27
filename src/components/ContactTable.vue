<template>
  <div id="contact-table">
    <p
      v-if="contacts.length < 1"
      class="empty-table"
    >
      No contacts
    </p>
    <table v-else>
      <thead>
        <tr>
          <th>Name</th>
          <th>Email</th>
          <th>Actions</th>
        </tr>
      </thead>
      <tbody>
        <tr
          :key="contact.id"
          v-for="contact in contacts"
        >
          <td v-if="editing === contact.id">
            <input
              type="text"
              v-model="contact.firstname"
            >
          </td>
          <td v-else>{{contact.firstname}}</td>
          <td v-if="editing === contact.id">
            <input
              type="text"
              v-model="contact.email"
            >
          </td>
          <td v-else>{{contact.email}}</td>
          <td v-if="editing === contact.id">
            <button @click="editContact(contact)">Save</button>
            <button
              class="muted-button"
              @click="editing = null"
            >Cancel</button>
          </td>
          <td v-else>
            <button @click="editMode(contact.id)">Edit</button>
            <button @click="$emit('delete:contact', contact.id)">Delete</button>
          </td>

        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: 'contact-table',
  props: {
    contacts: Array,
  },
  data() {
    return {
      editing: null,
    }
  },
  methods: {
    editMode(id) {
      this.editing = id
    },

    editContact(contact) {
      if (contact.name === '' || contact.email === '') return
      this.$emit('edit:contact', contact.id, contact)
      this.editing = null
    }
  }
}
</script>

<style scoped>
button {
  margin: 0 0.5rem 0 0;
}

input {
  margin: 0;
}

.empty-table {
  text-align: center;
}
</style>
