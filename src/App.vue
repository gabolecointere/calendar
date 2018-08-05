<template>
  <div id="app">
    <Form @form-submission='handleSubmission' />
  </div>
</template>

<script>
import Form from './components/Form.vue'
export default {
  name: 'app',
  components: {
    Form
  },
  data() {
    return {

    }
  },
  methods: {
    handleSubmission(payload) {
      alert(payload.days)
    }
  }
}
</script>

<style>
#app {
}
</style>
