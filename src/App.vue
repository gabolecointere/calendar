<template>
  <div id="app">
    <Form @form-submission='handleSubmission' />
    <Calendar v-if='showCalendar' :startFrom='startFrom' :days='days' :countryCode='countryCode'/>
  </div>
</template>

<script>
import Form from './components/Form.vue'
import Calendar from './components/Calendar.vue'

export default {
  name: 'app',
  components: {
    Form,
    Calendar
  },
  data() {
    return {
      startFrom: '',
      days: 0,
      countryCode: '',
      showCalendar: false
    }
  },
  methods: {
    handleSubmission(payload) {
      this.startFrom = payload.startFrom
      this.days = payload.days
      this.countryCode = payload.countryCode
      this.showCalendar = true
    }
  }
}
</script>

<style>
#app {
}
</style>
