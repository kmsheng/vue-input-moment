<template>
  <div id="app">
    <div class="time">{{moment.format('YYYY-MM-DD hh:mm:ss')}}</div>
    <InputMoment icon-prev-month="fa fa-arrow-left fa-fw"
                 icon-next-month="fa fa-arrow-right fa-fw"
                 :moment="moment"
                 :hour-step="1"
                 :min-step="1"
                 @change="handleCalendarChange" @save="handleSave" />
  </div>
</template>

<script>
import moment from 'moment';
import InputMoment from './components/InputMoment';

export default {
  name: 'app',
  data() {
    return {
      moment: moment()
    };
  },
  components: {
    InputMoment
  },
  methods: {
    handleCalendarChange(m) {

      const currentMonth = this.moment.clone().month();
      const nextMonth = m.clone().month();

      let dayInFuture = m;
      const today = moment().startOf('day');
      const alreadyPassed = (dayInFuture.clone().startOf('day').unix() < today.clone().unix());
      const monthChanged = ((currentMonth - nextMonth) !== 0);

      if (alreadyPassed && (! monthChanged)) {
        return;
      }

      if (alreadyPassed && monthChanged) {
        const hour = dayInFuture.clone().hour();
        const minute = dayInFuture.clone().minute();
        dayInFuture = today.clone().set({hour, minute});
      }

      this.moment = dayInFuture.clone();
    },
    handleSave() {
      console.log('saved !');
    }
  }
}
</script>

<style>
* {
  box-sizing: border-box;
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.time {
  padding: 20px 0;
}
</style>
