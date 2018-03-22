<template>
  <div class="time-pane">
    <div class="time-pane__watch-mirror">
      <span class="time-pane__watch-mirror-num">{{hour}}</span>
      <span class="time-pane__watch-mirror-separator">:</span>
      <span class="time-pane__watch-mirror-num">{{minute}}</span>
    </div>
    <div class="time-pane__label">Hours:</div>
    <range-slider class="time-pane__slider" min="0" max="23" :step="hourStep" v-model="hour" @change="handleHourChange" />
    <div class="time-pane__label">Minutes:</div>
    <range-slider class="time-pane__slider" min="0" max="59" :step="minStep" v-model="minute" @change="handleMinuteChange" />
  </div>
</template>

<script>
import RangeSlider from 'vue-range-slider';
import 'vue-range-slider/dist/vue-range-slider.css'

export default {
  name: 'TimePane',
  components: {
    RangeSlider,
  },
  props: {
    moment: Object,
    hourStep: {
      type: Number,
      default: 1
    },
    minStep: {
      type: Number,
      default: 1
    },
  },
  data() {
    const m = this.moment;
    return {
      hour: this.hour || m.hour(),
      minute: this.minute || m.minute()
    };
  },
  methods: {
    handleHourChange(newHour) {
      const m = this.moment.clone();
      this.$emit('change', m.set({
        'hour': newHour,
        'minute': m.minute()
      }));
    },
    handleMinuteChange(newMin) {
      const m = this.moment.clone();
      this.$emit('change', m.set({
        'hour': m.hour(),
        'minute': newMin
      }));
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .time-pane {
    height: 300px;
    padding-top: 60px;
    font-weight: 400;
  }
  .time-pane__watch-mirror-separator {
    font-size: 32px;
    width: 32px;
    height: 65px;
    color: #1385e5;
    display: inline-block;
  }
  .time-pane__watch-mirror-num {
    box-sizing: border-box;
    display: inline-block;
    background-color: #1385e5;
    color: #fff;
    font-size: 38px;
    border-radius: 3px;
    border: 1px solid #1385e5;
    width: 65px;
    height: 65px;
    line-height: 65px;
  }
  .time-pane__label {
    text-align: left;
    color: #1385e5;
    font-size: 15px;
    margin-top: 7px;
    margin-bottom: 10px;
  }
  .time-pane__slider {
    width: 100%;
  }
  .time-pane__slider :global(.range-slider-fill) {
    background-color: #1385e5;
  }
  .time-pane__slider :global(.range-slider-knob) {
    border: 3px solid #1385e5;
    box-shadow: none;
  }
</style>
