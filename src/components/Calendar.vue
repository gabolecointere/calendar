<template>
  <div class='calendar-container'> 
    <Month v-for='month in numMonths' :start='startFrom' :key='month.monthID' :countryCode='countryCode' :end='month.end' :year='month.year' :monthId='month.monthId' />
  </div>
</template>

<script>
import Mixin from './Mixin'
import Month from './Month'

const ONE_DAY = 86400000

export default {
  props: {
    'startFrom': {
      type: String,
      required: true
    },
    'days': {
      type: Number,
      required: true
    },
    'countryCode': { 
    type: String,
    required: true
    },
  },

  mixins: [Mixin],

  data() {
    return {
    }
  },

  components: {
    Month
  },

  computed: {
    begin() {
      return new Date(this.startFrom)
    },

    numMonths() {
      return this.calculateMonths(this.begin, this.begin, this.days);
    }
  },

  methods: {
    calculateMonths(p,d,n,m) {
      const finalDate = new Date(Date.parse(d) + (ONE_DAY * n));
      const prevDate = p;
      const currentDate = d;
      const num_days = n - 1;
      const arrMonths = m || [];
      if(this.dateComparator(finalDate, currentDate)) {
        arrMonths.push({monthId: currentDate.getMonth(), year: currentDate.getFullYear(), end: this.setDateFormat(currentDate)})
        return arrMonths;
      } else {

          if(!this.monthComparator(prevDate, currentDate)) {
              arrMonths.push({monthId: prevDate.getMonth(), year: prevDate.getFullYear(), end: this.setDateFormat(prevDate)});
          }
          return this.calculateMonths(currentDate, this.sumDays(currentDate), num_days, arrMonths);
        }
    },

    setDateFormat(date) {
      return (date.getFullYear() +'/'+ this.displayFormatMonth(date.getMonth()) +'/'+ this.displayFormatDay(date.getDate()))
    },

    displayFormatDay(d) {
      return (d < 10 ? "0"+d : d)
    },

    displayFormatMonth(m) {
      return (m+1 < 10 ? "0"+(m+1): m+1)
    }
  }
}
</script>

<style scoped>
.calendar-container {

    display: flex;
    justify-content: center;
    width: 1120px;
    margin: 10px auto;
    border: solid 1px #e3e3e3;
    flex-wrap: wrap;
}
</style>
