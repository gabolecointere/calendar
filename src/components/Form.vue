<template>
  <form class='form-container' id="form" v-on:submit.prevent='handleSubmit'>
        <div class='input-container'>
            <label>Start At:</label>
            <input type='date' id='start-from' v-model='startFrom' format='mm/dd/yyyy'  required />
        </div>
        <div class='input-container'>
            <label>Number of days:</label>
            <input type='number' id='days' v-model.number='days' min='0' required />
        </div>
        <div class='input-container'>
            <label>Country Code (ISO):</label>
            <input type='text' id='country-code' v-model='countryCode'  pattern='^[a-z]{2}|[A-Z]{2}$' placeholder='Example: us' required />
        </div>
        <div class='input-container'>
            <input type='submit' class='submit-btn' value='Submit'  />
        </div>
    </form>
</template>

<script>
export default {
  data() {
    return {
      startFrom: '',
      days: '',
      countryCode: ''
    }
  },
  methods: {
    handleSubmit() {
      let startFrom= this.startFrom
      let days= this.days
      let countryCode = this.countryCode
      let payload = Object.assign({}, {
        startFrom,
        days,
        countryCode
      })

      this.$emit('form-submission', payload)
    }
  }
}
</script>

<style scoped>
.form-container {
    display: flex;
    margin-top: 20px;
    align-items: center;
    width: 100%;
    justify-content: center;
}

.form-container .input-container {
    margin: 10px;
}

.input-container label {
    font-weight: bold;
    padding-right: 5px;
}

.input-container input {
    padding:10px;
    border: solid 1px #e3e3e3;
}

.submit-btn {
    background-color: #1d9e1d;
    color: white;
    font-weight: bolder;
    font-size: 13px;
    border-radius: 5px;
}

.submit-btn:focus {
    outline: 0;
}

</style>

