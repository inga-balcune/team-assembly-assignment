<template>
  <div>
    <form novalidate class="md-layout app-form" @submit.prevent="validateClient">
      <md-card class="md-layout-item md-size-100 md-small-size-100">
        <md-card-header>
          <div class="md-title">New Client</div>
        </md-card-header>

        <md-card-content>
          <div class="md-layout md-gutter">
            <div class="md-layout-item md-small-size-100">
              <md-field :class="getValidationClass('client')">
                <label for="Client">Client</label>
                <md-input name="client" id="client" autocomplete="client" v-model="form.client" :disabled="sending" />
                <span class="md-error" v-if="!$v.form.client.required">The client is required</span>
                <span class="md-error" v-else-if="!$v.form.client.minlength">Invalid client</span>
              </md-field>
            </div>

            <div class="md-layout-item md-small-size-100">
              <md-field :class="getValidationClass('logo')">
                <label for="logo">Logo link address</label>
                <md-input name="logo" id="logo" autocomplete="logo" v-model="form.logo" :disabled="sending" />
                <span class="md-error" v-if="!$v.form.logo.required">The logo is required</span>
                <span class="md-error" v-else-if="!$v.form.logo.minlength">Invalid logo</span>
              </md-field>
            </div>
          </div>

          <div class="md-layout md-gutter">
            <div class="md-layout-item md-small-size-100">
              <md-field :class="getValidationClass('firstName')">
                <label for="first-name">Contact Person Name</label>
                <md-input name="first-name" id="first-name" autocomplete="given-name" v-model="form.firstName" :disabled="sending" />
                <span class="md-error" v-if="!$v.form.firstName.required">The first name is required</span>
                <span class="md-error" v-else-if="!$v.form.firstName.minlength">Invalid first name</span>
              </md-field>
            </div>

            <div class="md-layout-item md-small-size-100">
              <md-field :class="getValidationClass('lastName')">
                <label for="last-name">Contact Person Surname</label>
                <md-input name="last-name" id="last-name" autocomplete="family-name" v-model="form.lastName" :disabled="sending" />
                <span class="md-error" v-if="!$v.form.lastName.required">The last name is required</span>
                <span class="md-error" v-else-if="!$v.form.lastName.minlength">Invalid last name</span>
              </md-field>
            </div>
          </div>

          <div class="md-layout md-gutter">
            <div class="md-layout-item md-small-size-100">
              <md-field :class="getValidationClass('number')">
                <label for="number">Contact Phone Number</label>
                <md-input id="number" name="number" autocomplete="number" v-model="form.number" :disabled="sending" />
                <span class="md-error" v-if="!$v.form.number.required">The number is required</span>
                <span class="md-error" v-else-if="!$v.form.number.maxlength">Invalid number</span>
              </md-field>
            </div>

            <div class="md-layout-item md-small-size-100">
              <md-field :class="getValidationClass('email')">
                <label for="email">Contact Email</label>
                <md-input type="email" name="email" id="email" autocomplete="email" v-model="form.email" :disabled="sending" />
                <span class="md-error" v-if="!$v.form.email.required">The email is required</span>
                <span class="md-error" v-else-if="!$v.form.email.email">Invalid email</span>
              </md-field>
            </div>
          </div>

        </md-card-content>

        <md-progress-bar md-mode="indeterminate" v-if="sending" />

        <md-card-actions>
          <md-button type="submit" class="md-primary" :disabled="sending">Save client</md-button>
        </md-card-actions>
      </md-card>
    </form>
  </div>
</template>

<script>
  import { validationMixin } from 'vuelidate'
  import {
    required,
    email,
    minLength,
    maxLength
  } from 'vuelidate/lib/validators'

  export default {
    name: 'FormValidation',
    mixins: [validationMixin],
    props: {
      sending: {
        type: Boolean,
        required: true
      },
      client: {
        type: Object,
        required: false
      }
    },
    data() {
      return {
        form: this.client ? {...this.client} : {
          client: null,
          firstName: null,
          lastName: null,
          number: null,
          email: null,
          logo: null,
        }
      }
    },
    validations: {
      form: {
        client: {
          required,
          minLength: minLength(3)
        },
        firstName: {
          required,
          minLength: minLength(3)
        },
        lastName: {
          required,
          minLength: minLength(3)
        },
        number: {
          required,
          maxLength: maxLength(8)
        },
        email: {
          required,
          email
        },
        logo: {
          required,
          minLength: minLength(3)
        }
      }
    },
    methods: {
      getValidationClass(fieldName) {
        const field = this.$v.form[fieldName]

        if (field) {
          return {
            'md-invalid': field.$invalid && field.$dirty
          }
        }
      },
      saveClient() {
        this.$emit('submit', this.form)
      },
      validateClient() {
        this.$v.$touch()

        if (!this.$v.$invalid) {
          this.saveClient()
        }
      }
    }
  }
</script>

<style lang="scss" scoped>
  .md-progress-bar {
    position: absolute;
    top: 0;
    right: 0;
    left: 0;
  }
</style>