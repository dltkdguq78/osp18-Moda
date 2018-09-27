<template>
  <div id="app">
    <TodoHeader></TodoHeader>
    <TodoInput v-on:addTodo="addTodo"></TodoInput>
    <!-- TodoList 의 Template 를 추가할 때 동작할 propdata 를 bind 한다.
     이후 completeTodo 메소드와 removeTodo 메소드에서 동작할 상위 함수를 bind한다. -->
    <TodoList v-bind:propsdata="todoItems" @completeTodo="completeTodo" @removeTodo="removeTodo"></TodoList>
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
		addTodo(todoItem) {
			localStorage.setItem(todoItem, {'context':todoItem, 'isComplete':false});
			this.todoItems.push({'context':todoItem, 'isComplete':false});
		},
    removeTodo(todoItem, index) {
      localStorage.removeItem(todoItem);
      this.todoItems.splice(index, 1);
    },
    // Todo 상태를 변경시키기 위한 함수
    completeTodo(todoItem, index){
      localStorage.setItem(todoItem, {'context':todoItem, 'isComplete':true});
      this.todoItems[index]['isComplete'] = true; //todoItems에 들어있는 Data의 Index를 조회하고 isComplete 키 값의 Value 데이터를 true로 변경시킨다.
    }
  },
  created() {
		if (localStorage.length > 0) {
			for (var i = 0; i < localStorage.length; i++) {
        // Dict (딕셔너리)) Key:Value
        // context : localStorage.Key(i) => 할일 문자열
        // isComplete : false => Boolean
				this.todoItems.push({'context':localStorage.key(i), 'isComplete':false});
        
			}
		}
  },
  components: {
    'TodoHeader': TodoHeader,
    'TodoInput': TodoInput,
    'TodoList': TodoList,
    'TodoFooter': TodoFooter
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
