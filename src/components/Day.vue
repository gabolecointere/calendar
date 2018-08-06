<template>
  <div :class='dayClass'>{{displayDate}}</div>
</template>

<script>
import Holidays from 'date-holidays'
const hd = new Holidays()

export default {
  props: {
    'type': {
      type: String,
      required: true
    },
    'date': {
      type: Number,
      required: true
    },
    'countryCode': {
      type: String,
      required: true
    },
    year: {

    },
    monthId: {

    }
  },
  data() {
    return {
      holiday: false
    }
  },
  computed: {
    displayDate() {
      return this.type === "gray" ? " ": this.date
    },
    dayClass() {
      if (this.holiday && this.displayDate !== " ") return "day holiday" 
      else return "day " + this.type
    },
  },

  created() {
    hd.init(this.countryCode)

    if (hd.isHoliday(new Date(this.year, this.monthId, this.date))) this.holiday = true
  }
}
</script>

<style scoped>
.holiday {
  background-color:aqua
}
</style>
