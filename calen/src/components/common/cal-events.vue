<template>
  <div class="events-wrapper">
    <h2 class="date" style="font-weight:normal">
      {{dayEventsTitle}}
    </h2>
    <div class="cal-events">
      <transition-group name="list">  
      <slot>
        <div v-for="(event, index) in events" class="event-item" :key="index">
          <cal-event-item :event="event" :index="index" :locale="locale" 
          @removeTodo="removeTodo" @completeTodo="completeTodo"></cal-event-item>
        </div>
      </slot>
    </transition-group> 
    </div>
  </div>
</template>

<script>
import kor from '../kor.js'
import { dateTimeFormatter } from '../tools.js'
import calEventItem from './cal-event-item.vue'
export default {
  name: 'cal-events',
  data () {
    return {
      kor
    }
  },
  components: {
    'cal-event-item': calEventItem
  },
  props: {
    title: String,
    dayEvents: {
      type: Object,
      required: true
    },
    locale: {
      type: String,
      required: true
    },
    color: {
      type: String,
      required: true
    }
  },
  computed: {
    dayEventsTitle () {
      if (this.title) return this.title
      if (this.dayEvents.date !== 'all') {
        let tempDate
        if (this.dayEvents.events.length !== 0) {
          tempDate = Date.parse(new Date(this.dayEvents.events[0].date))
          return dateTimeFormatter(tempDate, kor[this.locale].fullFormat)
        } else {
          tempDate = dateTimeFormatter(Date.parse(new Date(this.dayEvents.date)), kor[this.locale].fullFormat)
          return `${tempDate} ${kor[this.locale].notHaveEvents}`
        }
      } else {
        return kor[this.locale].dayEventsTitle
      }
    },
    events () {
      return this.dayEvents.events
    }
  },
  methods: {
    dateTimeFormatter,    
    completeTodo(event){
      this.$emit('completeTodo', event);
    },         
    removeTodo(todoItem, index) {
      this.$emit('removeTodo', todoItem, index);
    },
  }
}
</script>
<style>
   .list-enter-active, .list-leave-active {
      transition: all 1s;
   }
   .list-enter, .list-leave-to {
      opacity: 0;
      transform: translateY(30px);
   }
</style>
