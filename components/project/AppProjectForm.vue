<template>
  <div>
    <form novalidate class="md-layout app-form" @submit.prevent="validateProject">
      <md-card class="md-layout-item md-size-100 md-small-size-100">
        <md-card-header>
          <div class="md-title">Create New Project</div>
        </md-card-header>

        <md-card-content>
          <div class="md-layout md-gutter">
            <div class="md-layout-item md-small-size-100">
              <md-field :class="getValidationClass('title')">
                <label for="title">Project name</label>
                <md-input name="title" id="title" autocomplete="title" v-model="form.title" :disabled="sending" />
                <span class="md-error" v-if="!$v.form.title.required">The project name is required</span>
                <span class="md-error" v-else-if="!$v.form.title.minlength">Invalid project name</span>
              </md-field>
            </div>

          
            <div class="md-layout-item md-small-size-100">
              <md-field :class="getValidationClass('client')">
                <label for="client">Client</label>
                <md-select :name="form.client" :id="form.client" v-model="form.client" md-dense :disabled="sending">
                  <md-option value="" disabled selected></md-option>
                  <md-option v-for="(client, clientIndex) in clients" :value="client.id" :key="'A' + clientIndex">{{client.client}}</md-option>
                </md-select>
                <span class="md-error" v-if="!$v.form.client.required">The client is required</span>
                <span class="md-error" v-else-if="!$v.form.client.minlength">Invalid client</span>
              </md-field>
            </div>
          </div>

          <div class="md-layout md-gutter">
            <div class="md-layout-item md-small-size-100">
              <md-field :class="getValidationClass('projectManager')">
                <label for="projectManager">Choose a project manager</label>
                <md-select class="md-layout" :name="form.projectManager" :id="form.projectManager" v-model="form.projectManager" md-dense :disabled="sending">
                  <md-input class="md-layout-item md-large-size-100 input-field" v-model="search" @input='searchProjectManager'></md-input>
                  <md-option class="md-layout-item md-large-size-100" v-for="member in memberList" :value="member.id" :key="member.id">{{member.firstName}} {{member.lastName}}</md-option>
                </md-select>
                <span class="md-error" v-if="!$v.form.projectManager.required">The project manager is required</span>
              </md-field>
            </div>

            <div class="md-layout-item md-small-size-100">
              <md-field :class="getValidationClass('selectedTeam')">
                <label for="selectedTeam">Pick the project team</label>
                <md-select v-model="form.selectedTeam" md-dense :disabled="sending" multiple >
                  <md-optgroup v-for='(labelTitle, labelTitleIndex) in optionGroupLabel' :key="'C' + labelTitleIndex"  :label='`${labelTitle}s`'>
                    <md-option v-for="(employee, employeeIndex) in team" :key="'D' + employeeIndex"
                              v-show='labelTitle === employee.designation'
                              :value="employee.id"
                              :disabled="employee.id === form.projectManager">
                        {{employee.firstName}} {{employee.lastName}}
                    </md-option>
                  </md-optgroup>
                </md-select>
                <span class="md-error" v-if="!$v.form.selectedTeam.required">The client is required</span>
                <span class="md-error" v-if="!$v.form.selectedTeam.minLength">Team must consist on at least 2 members</span>
              </md-field>
            </div>
          </div>

          <div class="md-layout md-gutter">
            <div class="md-layout-item md-small-size-100">
              <md-field :class="getValidationClass('description')">
                <label for="description">Project description</label>
                <md-textarea name="description" id="description" v-model="form.description" :disabled="sending" />
                <span class="md-error" v-if="!$v.form.description.required">The description is required</span>
                <span class="md-error" v-else-if="!$v.form.description.minLength">Invalid description, must be at least 30 characters long</span>
              </md-field>
            </div>
          </div>

        </md-card-content>

        <md-progress-bar md-mode="indeterminate" v-if="sending" />

        <md-card-actions>
          <md-button type="submit" class="md-primary" :disabled="sending">Save project</md-button>
        </md-card-actions>
      </md-card>
    </form>
  </div>
</template>

<script>
  import axios from 'axios'
  import { validationMixin } from 'vuelidate'
  import {
    required,
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
      team: {
        type: Array,
        required: true
      },
      clients: {
        type: Array,
        required: true
      },
      project: {
        type: Object,
        required: false
      }
    },
    data() {
      return {
        form: this.project ? {...this.project} : {
          client: null,
          projectManager: null,
          selectedTeam: null,
          description: null,
          title: null,
        },
        search: '',
        memberList: null,
        optionGroupLabel: []
      }
    },
    validations: {
      form: {
        client: {
          required,
          minLength: minLength(3)
        },
        projectManager: {
          required,
          minLength: minLength(3)
        },
        selectedTeam: {
          required,
          minLength: minLength(2)
        },
        title: {
          required,
          minLength: minLength(3)
        },
        description: {
          required,
          minLength: minLength(30)
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
      saveProject() {
        this.$emit('submit', this.form)
      },
      validateProject() {
        this.$v.$touch()

        if (!this.$v.$invalid) {
          this.saveProject()
        }
      },
      searchProjectManager() {
        this.memberList = this.team.filter(member => {
          return member.firstName.toLowerCase().includes(this.search.toLowerCase()) || member.lastName.toLowerCase().includes(this.search.toLowerCase())
        })
      },
      // create a new option group label from options and remove duplicates
      setLabel() {
        this.team.forEach(member => {
          this.optionGroupLabel.push(member.designation)
        })
        this.optionGroupLabel = [...new Set(this.optionGroupLabel)]
      }
    },
    mounted() {
      this.memberList = this.team
      this.setLabel()
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