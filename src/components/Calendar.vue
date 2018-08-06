<template>
  <div class='calendar-container'> 
    <Month v-for='month in numMonths' :start='startFrom' :key='month.monthID' :code='countryCode' :end='month.end' :year='month.year' :monthId='month.monthId' />
           <!--  {numMonths.map(v => (<Month start={startAt} key={v.monthId} code={countryCode} {...v} /> ))} -->
  </div>
</template>

<script>
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

    dateComparator(d1, d2) {
      return (d1.getMonth() === d2.getMonth() && d1.getDate() === d2.getDate() && d1.getFullYear() === d2.getFullYear())
    },

    setDateFormat(date) {
      return (date.getFullYear() +'/'+ this.displayFormatMonth(date.getMonth()) +'/'+ this.displayFormatDay(date.getDate()))
    },

    displayFormatDay(d) {
      return (d < 10 ? "0"+d : d)
    },

    displayFormatMonth(m) {
      return (m+1 < 10 ? "0"+(m+1): m+1)
    },

    sumDays(d, n) {
            const days = typeof n === 'number' ? n * ONE_DAY: ONE_DAY
            return new Date(Date.parse(d) + days )
    },

    monthComparator(d1, d2) {
            return (d1.getMonth() === d2.getMonth() && d1.getFullYear() === d2.getFullYear())
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
