<template>
  <div :class='dayClass'>{{displayDate}}</div>
</template>

<script>
export default {
  props: {
    'type': {
      type: String,
      required: true
    },
    'date': {
      type: Number,
      required: true
    }
  },
  data() {
    return {

    }
  },
  computed: {
    displayDate() {
      return this.type === "gray" ? " ": this.date
    },
    dayClass() {
      return "day " + this.type
    }
  }
}
</script>

