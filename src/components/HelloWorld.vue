<template>
  <div class="hello">
    <CalendarWidget v-model="moment" @change="handleCalendarChange" />
  </div>
</template>

<script>
import 'vue-range-slider/dist/vue-range-slider.css';
import moment from 'moment';
import RangeSlider from 'vue-range-slider';
import CalendarWidget from './CalendarWidget';

export default {
  name: 'HelloWorld',
  data() {
    return {
      sliderValue: 0,
      moment: moment(),
    };
  },
  components: {
    RangeSlider,
    CalendarWidget
  },
  methods: {
    handleCalendarChange (m) {
      this.moment = m.clone();
    }
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .slider {
    width: 200px;
  }
</style>
