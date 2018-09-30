<template>
  <div id="app">
    <TodoHeader></TodoHeader>
    <TodoInput v-on:addTodo="addTodo"></TodoInput>
    <!-- TodoList 의 Template 를 추가할 때 동작할 propdata 를 bind 한다.
     이후 completeTodo 메소드와 removeTodo 메소드에서 동작할 상위 함수를 bind한다. -->
    <TodoList v-bind:propsdata="todoItems" @completeTodo="completeTodo" @removeTodo="removeTodo" @editTodo="editTodo"></TodoList>
    <TodoFooter v-on:removeAll="clearAll"></TodoFooter>
  </div>
</template>

<script>
import TodoHeader from './components/TodoHeader.vue'
import TodoInput from './components/TodoInput.vue'
import TodoList from './components/TodoList.vue'
import TodoFooter from './components/TodoFooter.vue'

export default {
  data() {
    return {
      todoItems: []
    }
  },
  methods: {
    clearAll() {
      localStorage.clear();
      this.todoItems = [];
    },
		addTodo(todoItem,context,date) {
      localStorage.setItem(todoItem, JSON.stringify({'title':todoItem, 'date': date,'context':context, 'isComplete':false, 'moreInfo':false}));
			this.todoItems.push({'title':todoItem,'context':context, 'date':date, 'isComplete':false, 'moreInfo':false});
		},
    removeTodo(todoItem, index) {
      localStorage.removeItem(todoItem.title);
      this.todoItems.splice(index, 1);
    },
    // Todo 상태를 변경시키기 위한 함수
    completeTodo(todoItem, index){
      todoItem.isComplete=!todoItem.isComplete;
		  localStorage.setItem(todoItem.title, JSON.stringify({'title':todoItem.title, 'date': todoItem.date,'context':todoItem.context, 'isComplete':todoItem.isComplete,'moreInfo':false}));
    },
	  editTodo(todoItem,index){
		   localStorage.setItem(todoItem.title, JSON.stringify({'title':todoItem.title, 'date': todoItem.date,'context':todoItem.context, 'isComplete':todoItem.isComplete,'moreInfo':false}));
	     }
  },
  created() {
		if (localStorage.length > 0) {
			for (var i = 0; i < localStorage.length; i++) {
        this.todoItems.push(JSON.parse(localStorage.getItem(localStorage.key(i))));
      }
	}
  },
  components: {
    'TodoHeader': TodoHeader,    'TodoInput': TodoInput,
    'TodoList': TodoList,    'TodoFooter': TodoFooter
  }
}
</script>

<style>
  body {
    text-align: center;
    background-color: #F6F6F8;
  }
  input {
    border-style: groove;
    width: 200px;
  }
  button {
    border-style: groove;
  }
  .shadow {
    box-shadow: 5px 10px 10px rgba(0, 0, 0, 0.03)
  }
</style>
