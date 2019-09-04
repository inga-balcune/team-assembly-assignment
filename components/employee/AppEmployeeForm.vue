<template>
  <div>
    <form novalidate class="md-layout app-form" @submit.prevent="validateUser">
      <md-card class="md-layout-item md-size-100 md-small-size-100">
        <md-card-header>
          <div class="md-title">Team Member</div>
        </md-card-header>

        <md-card-content>
          <div class="md-layout md-gutter">
            <div class="md-layout-item md-small-size-100">
              <md-field :class="getValidationClass('firstName')">
                <label for="first-name">First Name</label>
                <md-input name="first-name" id="first-name" autocomplete="given-name" v-model="form.firstName" :disabled="sending" />
                <span class="md-error" v-if="!$v.form.firstName.required">The first name is required</span>
                <span class="md-error" v-else-if="!$v.form.firstName.minlength">Invalid first name</span>
              </md-field>
            </div>

            <div class="md-layout-item md-small-size-100">
              <md-field :class="getValidationClass('lastName')">
                <label for="last-name">Last Name</label>
                <md-input name="last-name" id="last-name" autocomplete="family-name" v-model="form.lastName" :disabled="sending" />
                <span class="md-error" v-if="!$v.form.lastName.required">The last name is required</span>
                <span class="md-error" v-else-if="!$v.form.lastName.minlength">Invalid last name</span>
              </md-field>
            </div>
          </div>

          <div class="md-layout md-gutter">
            <div class="md-layout-item md-small-size-100">
              <md-field :class="getValidationClass('designation')">
                <label for="designation">Designation</label>
                <md-select name="designation" id="designation" v-model="form.designation" md-dense :disabled="sending">
                  <md-option disabled></md-option>
                  <md-option value="Consultant">CONSULTANT</md-option>
                  <md-option value="Innovation Lead">INNOVATION LEAD</md-option>
                  <md-option value="Strategist">STRATEGIST</md-option>
                  <md-option value="Designer">DESIGNER</md-option>
                  <md-option value="Back-end Developer">BACK-END DEVELOPER</md-option>
                  <md-option value="Front-end Developer">FRONT-END DEVELOPER</md-option>
                  <md-option value="Tech Team Lead">TECHNICAL TEAM LEAD</md-option>
                  <md-option value="Project Manager">PROJECT MANAGER</md-option>
                </md-select>
                <span class="md-error">The designation is required</span>
              </md-field>
            </div>

            <div class="md-layout-item md-small-size-100">
              <md-field :class="getValidationClass('number')">
                <label for="number">Phone number</label>
                <md-input id="number" name="number" autocomplete="number" v-model="form.number" :disabled="sending" />
                <span class="md-error" v-if="!$v.form.number.required">The number is required</span>
                <span class="md-error" v-else-if="!$v.form.number.maxlength">Invalid number</span>
              </md-field>
            </div>
          </div>

          <md-field :class="getValidationClass('email')">
            <label for="email">Email</label>
            <md-input type="email" name="email" id="email" autocomplete="email" v-model="form.email" :disabled="sending" />
            <span class="md-error" v-if="!$v.form.email.required">The email is required</span>
            <span class="md-error" v-else-if="!$v.form.email.email">Invalid email</span>
          </md-field>

          <md-field :class="getValidationClass('image')">
            <label for="image">Image link address</label>
            <md-input name="image" id="image" autocomplete="image" v-model="form.image" :disabled="sending" />
            <span class="md-error" v-if="!$v.form.image.required">The image is required</span>
            <span class="md-error" v-else-if="!$v.form.image.minlength">Invalid image</span>
          </md-field>

          <div class="md-layout md-gutter">
            <div class="md-layout-item md-small-size-100">
              <md-field :class="getValidationClass('background')">
                <label for="background">Background</label>
                <md-textarea name="background" id="background" v-model="form.background" :disabled="sending" />
                <span class="md-error" v-if="!$v.form.background.required">The background is required</span>
                <span class="md-error" v-else-if="!$v.form.background.minLength">Invalid background, must be at least 30 characters long</span>
              </md-field>
            </div>
          </div>

        </md-card-content>

        <md-progress-bar md-mode="indeterminate" v-if="sending" />

        <md-card-actions>
          <md-button type="submit" class="md-primary" :disabled="sending">Save team member</md-button>
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
      employee: {
        type: Object,
        required: false
      }
    },
    data() {
      return {
        form: this.employee ? {...this.employee} : {
        firstName: null,
        lastName: null,
        designation: null,
        number: null,
        email: null,
        image: null,
        background: null
        },
      }
    },
    validations: {
      form: {
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
        designation: {
          required
        },
        email: {
          required,
          email
        },
        image: {
          required,
          minLength: minLength(3)
        },
        background: {
          required,
          maxLength: minLength(30)
        }
      }
    },
    methods: {
      getValidationClass (fieldName) {
        const field = this.$v.form[fieldName]

        if (field) {
          return {
            'md-invalid': field.$invalid && field.$dirty
          }
        }
      },
      saveEmployee () {
        this.$emit('submit', this.form)
      },
      validateUser () {
        this.$v.$touch()

        if (!this.$v.$invalid) {
          this.saveEmployee()
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