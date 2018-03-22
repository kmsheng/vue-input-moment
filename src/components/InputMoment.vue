<template>
  <div class="input-moment">
    <div class="input-moment__options">
      <button :class="[{'active': isDatePaneActive},'input-moment__btn-option']"
              @click.prevent="setActiveTab('date')">Date</button>
      <button :class="[{'active': isTimePaneActive},'input-moment__btn-option']"
              @click.prevent="setActiveTab('time')">Time</button>
    </div>
    <DatePane v-if="isDatePaneActive"
              :moment="moment"
              :icon-prev-month="iconPrevMonth"
              :icon-next-month="iconNextMonth"
              @change="handleCalendarChange" />
    <TimePane v-else :moment="moment" :hour-step="hourStep" :min-step="hourStep" @change="handleCalendarChange" />
    <button type="button" class="input-moment__btn-save" @click.prevent="handleSave">Save</button>
  </div>
</template>

<script>
import DatePane from './DatePane';
import TimePane from './TimePane';

export default {
  name: 'InputMoment',
  props: {
    moment: Object,
    iconPrevMonth: String,
    iconNextMonth: String,
    hourStep: {
      type: Number,
      default: 1
    },
    minStep: {
      type: Number,
      default: 1
    },
    defaultActiveTab: {
      type: String,
      default: 'time'
    },
  },
  data () {
    return {
      activeTab: this.activeTab || this.defaultActiveTab
    };
  },
  components: {
    DatePane,
    TimePane
  },
  computed: {
    isDatePaneActive() {
      return (this.activeTab === 'date');
    },
    isTimePaneActive() {
      return (this.activeTab === 'time');
    }
  },
  methods: {
    handleCalendarChange(m) {
      this.$emit('change', m);
    },
    setActiveTab(value) {
      this.activeTab = value;
    },
    handleSave() {
      this.$emit('save');
    }
  },
}
</script>

<style scoped>
.input-moment {
  display: inline-block;
  padding: 12px 15px;
  width: 330px;
  border: 1px solid #dfe0e4;
  border-radius: 3px;
}
.input-moment__btn-option:first-child {
  border-top-right-radius: 0;
  border-bottom-right-radius: 0;
}
.input-moment__btn-option:last-child {
  border-top-left-radius: 0;
  border-bottom-left-radius: 0;
}
.input-moment__btn-option {
  cursor: pointer;
  padding: 7px;
  font-size: 16px;
  width: 50%;
  color: #1385e5;
  background-color: #fff;
  border: 1px solid #1385e5;
  border-radius: 3px;
}
.input-moment__btn-option.active {
  color: #fff;
  background-color: #1385e5;
}
.input-moment__btn-save {
  display: block;
  width: 100%;
  background-color: #1385e5;
  color: #fff;
  font-size: 16px;
  padding: 7px 0;
  border: 1px solid #1385e5;
  border-radius: 3px;
  margin-top: 34px;
}
</style>
