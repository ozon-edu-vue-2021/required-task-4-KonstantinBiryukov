<template>
  <div class="form">
    <span>Личные данные</span>
    <div class="row">
      <label for="name">
        Name
      </label>
      <input
          v-model="formData.name"
          type="text"
          id="name"
          placeholder="name"
      >
      <label for="surname">
        Surname
      </label>
      <input
          v-model="formData.surname"
          type="text"
          id="surname"
          placeholder="surname"
      >
      <label for="patronymic">
        Patronymic
      </label>
      <input
          v-model="formData.patronymic"
          type="text"
          id="patronymic"
          placeholder="patronymic"
      >
    </div>

    <div class="row">
      <label for="birthDate">
        Date of birth
      </label>
      <input
          v-model="formData.birthDate"
          type="text"
          id="birthDate"
          placeholder="birthDate"
      >
    </div>

    <div class="row">
      <label for="email">
        Email
      </label>
      <input
          v-model="formData.email"
          type="text"
          id="email"
          placeholder="test@gmail.com"
      >
    </div>
  </div>
</template>

<script>
import clickOutside from "vue-click-outside"

export default {
  name: "Form",
  directives: {
    clickOutside
  },
  data() {
    return {
      formData: {
        name: "",
        surname: "",
        patronymic: "",
        birthDate: "",
        email: "",
        gender: "",
      }
    };
  },
  methods: {},
};
</script>

<style scoped>
.form {
}
</style>
