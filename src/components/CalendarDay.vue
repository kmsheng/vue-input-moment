<template>
  <td :class="dayClass" @click="handleClick">{{i}}</td>
</template>

<script>

export default {
  name: 'CalendarDay',
  props: {
    i: Number,
    w: Number,
    d: Number
  },
  computed: {
    dayClass () {
      const {i, w, d} = this;
      const prevMonth = (w === 0) && (i > 7);
      const nextMonth = (w >= 4) && (i <= 14);
      return {
        'calendar-day__prev-month': prevMonth,
        'calendar-day__next-month': nextMonth,
        'calendar-day__current-day': (! prevMonth) && (! nextMonth) && (i === d)
      };
    },
  },
  methods: {
    handleClick (event) {
      this.$emit('click', event);
    },
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.calendar-day__prev-month,
.calendar-day__next-month  {
  color: #999;
}
.calendar-day__current-day {
  color: #1385e5;
  font-weight: bold;
}
</style>
