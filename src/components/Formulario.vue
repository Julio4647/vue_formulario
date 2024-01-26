<template>
    <form @submit.prevent="validateForm">
      <div>
        <label for="fullName">Nombre completo:</label>
        <input type="text" id="fullName" v-model.trim="fullName" @blur="validateName" />
        <p v-if="nameError" class="error">{{ nameError }}</p>
      </div>
      <div>
        <label for="address">Dirección:</label>
        <input type="text" id="address" v-model.trim="address" @blur="validateAddress" />
        <p v-if="addressError" class="error">{{ addressError }}</p>
      </div>
      <div>
        <label for="birthdate">Fecha de nacimiento:</label>
        <input type="date" id="birthdate" v-model="birthdate" @blur="validateBirthdate" />
        <p v-if="birthdateError" class="error">{{ birthdateError }}</p>
      </div>
      <div>
        <label for="email">Correo electrónico:</label>
        <input type="email" id="email" v-model.trim="email" @blur="validateEmail" />
        <p v-if="emailError" class="error">{{ emailError }}</p>
      </div>
      <div>
        <label for="phone">Número telefónico:</label>
        <input type="tel" id="phone" v-model.trim="phone" @blur="validatePhone" />
        <p v-if="phoneError" class="error">{{ phoneError }}</p>
      </div>
      <div>
        <label for="image">Fotografía (PNG):</label>
        <input type="file" id="image" @change="validateImage" />
        <p v-if="imageError" class="error">{{ imageError }}</p>
      </div>
      <button type="submit">Enviar</button>
    </form>
  </template>
  
  <script>
  export default {
    data() {
      return {
        fullName: '',
        address: '',
        birthdate: '',
        email: '',
        phone: '',
        image: '',
        nameError: '',
        addressError: '',
        birthdateError: '',
        emailError: '',
        phoneError: '',
        imageError: ''
      }
    },
    methods: {
      validateName() {
        if (!this.fullName.includes(' ')) {
          this.nameError = 'Por favor, ingrese un nombre y apellido.'
        } else {
          const names = this.fullName.split(' ')
          if (names.length < 2) {
            this.nameError = 'Por favor, ingrese un nombre y apellido.'
          } else {
            this.nameError = ''
          }
        }
      },
      validateAddress() {
        const addressL = this.address
        const direccionRegex = /^\d+\s[A-z]+\s\d+,\s[A-z]+$/;
        if (!direccionRegex.test(addressL)) {
            this.addressError.push('Formato de dirección inválido.');
        } else {
            this.addressError = ''
        }
      },
      validateBirthdate() {
        const today = new Date()
        const birthdate = new Date(this.birthdate)
        if (birthdate > today || birthdate > new Date(today.getFullYear() - 18, today.getMonth(), today.getDate())) {
          this.birthdateError = 'La fecha de nacimiento no puede ser en el futuro o mayor a 18 años atrás.'
        } else {
          this.birthdateError = ''
        }
      },
      validateEmail() {
        const emailL = this.email
        const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
        if (!emailRegex.test(emailL)) {
            this.emailError.push('Formato de correo electrónico inválido.');
        } else {
            this.emailError = ''
        }
      },
      validatePhone() {
        const telefonoLength = this.phone.length; // Eliminar caracteres no numéricos
        if (telefonoLength !== 10) {
          this.phoneError.push('El número telefónico debe tener 10 dígitos.');
        } else {
            this.phoneError = ''
        }
      },
      validateImage(event) {
        const file = event.target.files[0]
        if (file.type !== 'image/png') {
          this.imageError = 'Por favor, seleccione una imagen PNG.'
        } else if (file.size > 3000000) {
          this.imageError = 'El tamaño de la imagen no debe exceder los 3 MB.'
        } else {
          this.imageError = ''
        }
      },
      validateForm() {
        this.validateName()
        this.validateAddress()
        this.validateBirthdate()
        this.validateEmail()
        this.validatePhone()
        this.validateImage()
        if (!this.nameError && !this.addressError && !this.birthdateError && !this.emailError && !this.phoneError && !this.imageError) {
        }
      }
    }
  }
  </script>
  
  <style>
  .error {
    color: red;
  }
  </style>