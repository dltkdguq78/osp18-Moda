<template>
  <div id="app">
    <div class="main">
      <div class="calendar-holder">
        <calendar :events="events" />
      </div>
    </div>
  </div>
</template>

<script>
import Calendar from './common/Calendar.vue'
import Pusher from 'pusher-js';

export default {
  name: 'app',
  components: {
    Calendar
  },
  data(){
    return {
      events: [{
        title     :  'event1',
        start     : '2018-07-09',
        cssClass  : 'blue',
        YOUR_DATA : {}
      },
      {
        title     : 'event2',
        start     : '2018-07-10',
        end       : '2018-07-13',
        cssClass  : ['orange']
      }] 
    }
  },
  created(){
    // Add your pusher credentials
    const pusher = new Pusher('PUSHER_KEY', {
      cluster: 'PUSHER_CLUSTER',
      encrypted: true,
    });
    const channel = pusher.subscribe('schedule');
    channel.bind('new-event', (data) => {
      this.events = [
        ...this.events,
        data
      ]
    })
  }
}
</script>

<style>
#app {
  text-align: center;
  margin-top:15px;
  margin-bottom: 15px;
}
.main {
  display: flex;
  align-items: center;
}
.calendar-holder {
  width: 100%;
}
</style>
