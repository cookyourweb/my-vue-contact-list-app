<template>
  <div id="contact-form">
    <form @submit.prevent="handleSubmit">

      <label>Contact name</label>
      <input
        ref="first"
        type="text"
        :class="{ 'has-error': submitting && invalidName }"
        v-model="contact.firstname"
        @focus="clearStatus"
        @keypress="clearStatus"
      >
      <label>Contact Email</label>
      <input
        type="text"
        :class="{ 'has-error': submitting && invalidEmail }"
        v-model="contact.email"
        @focus="clearStatus"
      >
      <p
        v-if="error && submitting"
        class="error-message"
      >❗Please fill out all required fields</p>
      <p
        v-if="success"
        class="success-message"
      >✅ Contact successfully added</p>
      <button>Add Contact</button>
    </form>

  </div>
</template>

<script>
export default {
  firstname: 'contact-form',
  data() {
    return {
      error: false,
      submitting: false,
      success: false,
      contact: {
        firstname: '',
        email: '',
      }
    }
  },
  computed: {
    invalidName() {
      return this.contact.firstname === ''
    },

    invalidEmail() {
      return this.contact.email === ''
      
    },
  },
  methods: {
    handleSubmit() {
      this.clearStatus()
      this.submitting = true

      if (this.invalidName || this.invalidEmail) {
        this.error = true
        return
      }

      this.$emit('add:contact', this.contact)
      this.$refs.first.focus()
      this.contact = {
        firstname: '',
        email: '',
      }
      this.success = true
      this.error = false
      this.submitting = false
    },

    clearStatus() {
      this.success = false
      this.error = false
    }
  }}
</script>

<style scoped>
form {
  margin-bottom: 2rem;
}

[class*="-message"] {
  font-weight: 500;
}

.error-message {
  color: #d33c40;
}

.success-message {
  color: #32a95d;
}
</style>
