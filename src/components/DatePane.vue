<template>
  <div class="date-pane">
    <div class="date-pane__toolbar">
      <button class="date-pane__btn-prev" type="button" @click.prevent="goToPrevMonth">
        <i :class="iconPrevMonth" />
      </button>
      <span class="date-pane__current-date">{{moment.format('MMMM YYYY')}}</span>
      <button class="date-pane__btn-next" type="button" @click.prevent="goToNextMonth">
        <i :class="iconNextMonth" />
      </button>
    </div>
    <table>
      <thead>
        <tr>
          <td v-for="(week, index) in weeks" :key="index">{{week}}</td>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(row, w) in rows" :key="w">
          <CalendarDay v-for="(i) in row" :key="i" :i="i" :w="w" :d="d" @click="selectDate(i, w)" />
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import range from 'lodash/range';
import chunk from 'lodash/chunk';
import CalendarDay from './CalendarDay';

export default {
  name: 'DatePane',
  props: {
    moment: Object,
    iconPrevMonth: String,
    iconNextMonth: String,
  },
  components: {
    CalendarDay,
  },
  data () {
    return {
      weeks: ['Sun', 'Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat']
    };
  },
  computed: {
    m () {
      return this.moment;
    },
    d () {
      return this.m.date();
    },
    days () {
      const {m} = this;
      const d1 = m.clone().subtract(1, 'month').endOf('month').date();
      const d2 = m.clone().date(1).day();
      const d3 = m.clone().endOf('month').date();
      return [].concat(
        range(d1 - d2 + 1, d1 + 1),
        range(1, d3 + 1),
        range(1, 42 - d3 - d2 + 1)
      );
    },
    rows () {
      return chunk(this.days, 7);
    }
  },
  methods: {
    selectDate (i, w) {
      const prevMonth = (w === 0) && (i > 7);
      const nextMonth = (w >= 4) && (i <= 14);
      const hour = this.moment.clone().hour();
      const minute = this.moment.clone().minute();
      const m = this.moment.clone();
      if (prevMonth) {
        m.subtract(1, 'month');
      }
      if (nextMonth) {
        m.add(1, 'month');
      }
      m.date(i);
      m.set({hour, minute});
      this.$emit('change', m);
    },
    goToPrevMonth () {
      this.$emit('change', this.moment.clone().subtract(1, 'month'));
    },
    goToNextMonth () {
      this.$emit('change', this.moment.clone().add(1, 'month'));
    }
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .date-pane {
    width: 100%;
    display: inline-block;
    height: 300px;
  }
  .date-pane table {
    border-collapse: collapse;
    width: 100%;
  }
  .date-pane thead td {
    color: #1385e5;
    font-weight: 700;
    text-transform: uppercase;
    font-size: 12px;
  }
  .date-pane td {
    width: 32px;
    cursor: pointer;
    border: 1px solid #dfe0e4;
    padding: 8px 0;
    text-align: center;
  }
  .date-pane__toolbar {
    display: flex;
    justify-content: space-between;
    padding: 12px 0;
  }
  .date-pane__btn-prev,
  .date-pane__btn-next {
    cursor: pointer;
    display: flex;
    justify-content: center;
    width: 30px;
    height: 30px;
    font-size: 20px;
    background-color: #1385e5;
    border: 1px solid #1385e5;
    border-radius: 100%;
    color: #fff;
  }
  .date-pane__current-date {
    line-height: 30px;
    color: #1385e5;
    font-size: 14px;
  }
</style>
