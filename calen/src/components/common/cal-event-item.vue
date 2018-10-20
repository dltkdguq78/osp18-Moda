<template>
  <div class="wrapper">
    <h3 class="title">
      <div v-if="event.isComplete == false"  @click="completeTodo(event)">
        <div v-if="event.picked=='important'">
          <i class="checkBtn fas fa-check" style = "color:#FFBF00" aria-hidden="true"></i>
            {{ event.title }}
        </div>
        <div v-else-if="event.picked == 'emergency'">
          <i class="checkBtn fas fa-check" style = "color:#FF0000" aria-hidden="true"></i><!--젤왼쪽 [checkbtn] -->
            {{ event.title }}
        </div>
        <div v-else>
          <i class="checkBtn fas fa-check" style = "color:#62acde" aria-hidden="true"></i>
            {{ event.title }}
        </div>
      </div>
      <div v-else>
        <i class="checkBtn fas fa-check" style = "color:#A4A4A4" 
            aria-hidden="true" @click="completeTodo(event)"></i>
          <del>{{ event.title }}</del>
      </div>      
    </h3>
    <p class="time">{{dateTimeFormatter(Date.parse(new Date(event.date)),kor[locale].fullFormat)}}</p>
    <span class="removeBtn" type="button" @click="removeTodo(event, index)"><!--editbnt , [휴지통] -->
        <i class="far fa-trash-alt" aria-hidden="true"></i>
    </span>
    <div v-if="event.context==''">
      <br>
    </div>
    <p class="desc">{{event.context}}
    </p>
  </div>
</template>
<script>
import kor from '../kor.js'
import { dateTimeFormatter } from '../tools.js'
export default {
  data () {
    return {
      kor
    }
  },
  props: {
    event: {
      type: Object,
      required: true
    },
    index: {
      type: Number,
      required: true
    },
    locale: {
      type: String,
      required: true
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
   .removeBtn {
      float: right;
      color: #de4343;
      margin-top: 6px;
   }
</style>
