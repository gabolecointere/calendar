<script>
const ONE_DAY = 86400000

export default {
  data() {
    return {

    }
  },

  methods: {
    dateComparator(d1, d2) {
      return (d1.getMonth() === d2.getMonth() && d1.getDate() === d2.getDate() && d1.getFullYear() === d2.getFullYear())
    },

    sumDays(d, n) {
            const days = typeof n === 'number' ? n * ONE_DAY: ONE_DAY
            return new Date(Date.parse(d) + days )
    },

    monthComparator(d1, d2) {
            return (d1.getMonth() === d2.getMonth() && d1.getFullYear() === d2.getFullYear())
    },
  }
}
</script>
