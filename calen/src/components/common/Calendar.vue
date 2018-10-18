<template>
  <full-calendar :events="events" />
</template>

<script>
import FullCalendar from 'vue-fullcalendar';

export default {
  name: 'Calendar',
  props: ['events'],
  components: {
    FullCalendar
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
.event-item {
  padding: 2px 0 2px 4px !important;
}
</style>
