<template>
  <div class="show_application">

    <template v-if="application">
      <h1>Application</h1>

      <h2>Application details</h2>

      <div
        v-for="(field, index) in application.properties.form_data"
        :key="`field_${index}`">
        <span>{{field.label}}</span>
        <span>{{field.value}}</span>
      </div>

      <h2>Approval flow</h2>

      <div
        v-for="(recipient, index) in application.recipients"
        :key="`recipient_${index}`">
        <div class="">
          <span>{{recipient.properties.display_name}}</span>
        </div>

        <div class="" v-if="recipient.approval">
          Approval: {{recipient.approval.identity}}
        </div>

        <div class="" v-if="recipient.refusal">
          Refusal: {{recipient.refusal.identity}}
        </div>


      </div>


    </template>

  </div>
</template>

<script>

import CurrentUserID from '@/mixins/CurrentUserID.js'

export default {
  name: 'ShowApplication',
  components: {

  },
  mixins: [
    CurrentUserID
  ],
  mounted () {
    this.get_application()
  },
  data () {
    return {

      loading: false,
      error: null,

      // experimental
      application: null,


    }
  },
  watch: {
    application_id () {
      this.get_application()
    }
  },
  methods: {
    get_application () {
      // Get the body of the application, regardless of its recipients
      // This gets the applicant as well (for the time being)

      this.loading = true

      const url = `${process.env.VUE_APP_SHINSEI_MANAGER_URL}/v2/applications/${this.application_id}`
      this.axios.get(url)
        .then(({data}) => {
          // Form data is stored as a stringified JSON in the DB
          data.properties.form_data = JSON.parse(data.properties.form_data)
          this.application = data
        })
        .catch((error) => {
          console.error(error)
          this.error = error
        })
        .finally(() => { this.loading = false })
    },

  },
  computed: {
    application_id(){
      return this.$route.params.application_id
    }

  }
}
</script>

<style scoped>


</style>
