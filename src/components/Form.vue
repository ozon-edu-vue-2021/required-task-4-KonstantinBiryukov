<template>
  <v-form
      lazy-validation
      ref="form">
    <v-container>
      <span>Personal Data</span>
      <v-row>
        <v-col cols="12" md="4">
          <v-text-field
              v-model="personalData.name"
              :counter="20"
              :rules="[rules.required, rules.cyrillic]"
              label="First name"
              required
              solo
          ></v-text-field>
        </v-col>

        <v-col cols="12" md="4">
          <v-text-field
              v-model="personalData.surname"
              :counter="20"
              :rules="[rules.required, rules.cyrillic]"
              label="Last name"
              required
              solo
          ></v-text-field>
        </v-col>

        <v-col cols="12" md="4">
          <v-text-field
              v-model="personalData.patronymic"
              :counter="20"
              :rules="[rules.required, rules.cyrillic]"
              label="Patronymic"
              required
              solo
          ></v-text-field>
        </v-col>
      </v-row>

      <v-row>
        <v-col cols="12" md="10">
          <v-text-field
              v-model="personalData.birthDate"
              :rules="[rules.required, rules.date]"
              :counter="10"
              label="Date of birth"
              placeholder="DD.MM.YYYY"
              required
              solo
          ></v-text-field>
        </v-col>
      </v-row>

      <v-row>
        <v-col cols="12" md="10">
          <v-text-field
              v-model="personalData.email"
              :rules="[rules.required, rules.email]"
              :counter="30"
              label="Email"
              required
              solo
          ></v-text-field>
        </v-col>
      </v-row>

      <v-radio-group
          v-model="personalData.gender"
          row
          required
          :rules="[rules.required]"
      >
        <v-radio
            label="Male"
            value="male"
        ></v-radio>
        <v-radio
            label="Female"
            value="female"
        ></v-radio>
      </v-radio-group>

      <span> Passport data </span>

      <v-col
          class="d-flex"
          cols="12"
          sm="6"
      >
        <v-autocomplete
            :items="citizenships"
            label="Citizenship"
            :search-input.sync="searchInput"
            dense
            outlined
        ></v-autocomplete>
      </v-col>

      <div v-if="!passportData.citizenship">

      </div>
      <div v-else>
        <div v-if="passportData.citizenship=== 'Russia'">
          <v-row>
            <v-col cols="12" md="4">
              <v-text-field
                  v-model="passportData.passportSerial"
                  :counter="4"
                  :rules="[rules.passportSerial, rules.required]"
                  label="Passport Serial"
                  required
                  solo
              ></v-text-field>
            </v-col>

            <v-col cols="12" md="4">
              <v-text-field
                  v-model="passportData.passportNumber"
                  :counter="6"
                  :rules="[rules.passportNumber, rules.required]"
                  label="Passport Number"
                  required
                  solo
              ></v-text-field>
            </v-col>

            <v-col cols="12" md="4">
              <v-text-field
                  v-model="passportData.issueDate"
                  :rules="[rules.date]"
                  :counter="10"
                  label="Issue date"
                  required
                  solo
              ></v-text-field>
            </v-col>
          </v-row>
        </div>

        <div v-else>
          <v-row>
            <v-col cols="12" md="6">
              <v-text-field
                  v-model="foreignPassport.latinName"
                  :counter="20"
                  :rules="[rules.required, rules.english]"
                  label="Latin Name"
                  required
                  solo
              ></v-text-field>
            </v-col>

            <v-col cols="12" md="6">
              <v-text-field
                  v-model="foreignPassport.latinSurname"
                  :rules="[rules.required, rules.english]"
                  :counter="20"
                  label="Latin Surname"
                  required
                  solo
              ></v-text-field>
            </v-col>
          </v-row>

          <v-row>
            <v-col cols="12" md="4">
              <v-text-field
                  v-model="passportData.passportNumber"
                  :counter="15"
                  label="Passport Number"
                  required
                  solo
              ></v-text-field>
            </v-col>

            <v-col
                cols="12"
                md="4"
            >
              <v-select
                  :items="citizenships"
                  label="Country of Issue"
                  v-model="foreignPassport.countryIssue"
                  dense
                  outlined
              ></v-select>
            </v-col>

            <v-col
                cols="12"
                md="4"
            >
              <v-select
                  :items="passportTypes"
                  label="Passport Type"
                  v-model="foreignPassport.passportType"
                  dense
                  outlined
              ></v-select>
            </v-col>

          </v-row>
        </div>
      </div>

      <div>
        <span>Have you ever changed your name?</span>
        <v-radio-group
            v-model="changeName.wasNameChanged"
            row
        >
          <v-radio
              label="Yes"
              value="yes"
          ></v-radio>
          <v-radio
              label="No"
              value="no"
          ></v-radio>
        </v-radio-group>

        <v-row v-if="changeName.wasNameChanged === 'yes'">
          <v-col cols="12" md="6">
            <v-text-field
                v-model="changeName.firstNamePrevious"
                :counter="20"
                :rules="[rules.cyrillic]"
                label="First name"
                solo
            ></v-text-field>
          </v-col>

          <v-col cols="12" md="6">
            <v-text-field
                v-model="changeName.lastNamePrevious"
                :counter="20"
                :rules="[rules.cyrillic]"
                label="Last name"
                solo
            ></v-text-field>
          </v-col>
        </v-row>
      </div>
      <v-btn
          class="mr-4"
          @click="validate()"
      >
        submit
      </v-btn>

    </v-container>
  </v-form>
</template>

<script>
import citizenships from "@/assets/data/citizenships.json";
import passportTypes from "@/assets/data/passport-types.json";
import {debounce} from "@/helpers/debounce";

export default {
  name: "Form",
  data() {
    return {
      personalData: {
        name: "",
        surname: "",
        patronymic: "",
        birthDate: "",
        email: "",
        gender: "",
      },
      passportData: {
        citizenship: "",
        passportSerial: "",
        passportNumber: "",
        issueDate: "",
      },
      foreignPassport: {
        latinName: "",
        latinSurname: "",
        countryIssue: "",
        passportType: ""
      },
      changeName: {
        wasNameChanged: "no",
        firstNamePrevious: "",
        lastNamePrevious: ""
      },
      citizenships: [],
      passportTypes: [],
      rules: {
        required: value => !!value || 'This field is required',
        cyrillic: value => {
          const pattern = /^[аАбБвВгГдДеЕёЁжЖзЗиИйЙкКлЛмМнНоОпПрРсСтТуУфФхХцЦчЧшШщЩъЪыЫьЬэЭюЮяЯ]+$/;
          return pattern.test(value) || `Only cyrillic symbols are allowed`;
        },
        english: value => {
          const pattern = /^[a-zA-Z]+$/;
          return pattern.test(value) || `Only latin symbols are allowed`;
        },
        passportSerial: value => {
          const pattern = /^\d{4}$/;
          return pattern.test(value) || `Only 4 digits are allowed`;
        },
        passportNumber: value => {
          const pattern = /^\d{6}$/;
          return pattern.test(value) || `Only 6 digits are allowed`;
        },
        email: value => {
          const pattern = /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
          return pattern.test(value) || `Email is not valid`;
        },
        date: value => {
          const pattern = /^([0-9]{2}).([0-9]{2}).([0-9]{4})$/;
          let isValid = pattern.test(value);
          if (isValid && new Date(value) >= new Date()) {
            return `Date cannot be greater than the current time`;
          }
          return isValid || `Date  is not valid`;
        }
      },
      searchInput: ""
    };
  },
  created() {
    this.citizenships = citizenships.map(citizenship => citizenship.nationality);
    this.passportTypes = passportTypes.map(passportType => passportType.type);
  },
  watch: {
    searchInput: debounce(function (newVal) {
      if (newVal !== null) {
        this.passportData.citizenship = newVal;
        console.log(newVal);
      }
    }, 2000)
  },
  methods: {
    validate() {
      const isValid = this.$refs.form.validate();
      if (isValid) {
        console.log("personal data", this.personalData);
        console.log("passport data", this.passportData);
        console.log("foreign passport", this.foreignPassport);
        console.log("name change", this.changeName);

        alert("Form is submitted");
      }
    },
  },
};
</script>

<style>
</style>
