<template>
  <div id="app">
    <TodoHeader></TodoHeader>
    <TodoInput v-on:addTodo="addTodo" v-on:TagremoveTodo="TagremoveTodo" 
        v-on:searchTodo="searchTodo" @sortTodo ="sortTodo"></TodoInput>
    <!-- TodoList 의 Template 를 추가할 때 동작할 propdata 를 bind 한다.
     이후 completeTodo 메소드와 removeTodo 메소드에서 동작할 상위 함수를 bind한다. -->
    <TodoList v-bind:propsdata="todoItems" v-bind:search="search" @completeTodo="completeTodo"
          @removeTodo="removeTodo" @editTodo="editTodo"></TodoList>
      <!-- List에 search값 바인딩 시키면 search 기능 완성  -->    
    <TodoFooter v-on:removeAll="clearAll"></TodoFooter>
    <TodoAlert v-bind:propsdata="todoItems" @completeTodo="completeTodo"></TodoAlert>
  </div>
</template>

<script>
import TodoHeader from './components/TodoHeader.vue'
import TodoInput from './components/TodoInput.vue'
import TodoList from './components/TodoList.vue'
import TodoFooter from './components/TodoFooter.vue'
import TodoAlert from './components/TodoAlert.vue'

export default {  
  data() {
    return {
      search:'',
      list:true,
      todoItems: []
    }
  },
  methods: {
    clearAll() {
      localStorage.clear();
      this.todoItems = [];
    },
    // title
      addTodo(key, context, date, picked) { //Input에서 상위컴포넌트인 App으로 스트링을 넘겨줌.
      //console.log(key);
      localStorage.setItem(key, JSON.stringify({'title':key, 'date': date,'context':context, 'isComplete':false, 'moreInfo':false, 'picked':picked})); // 키값은 todoItem,  Json으로 객체화시켜 스트링값을 넘겨줌
      this.todoItems.push({'title':key,'context':context, 'date':date, 'isComplete':false, 'moreInfo':false, 'picked':picked});//todoItem.title = todoItem , todoItem.context = context , todoItem.date = date , todoItem.isComplete = false
      },

    // {'title': v , 'date':v, 'context':v, 'isComplete':v}
      removeTodo(todoItem, index) {
      localStorage.removeItem(todoItem.title);
      this.todoItems.splice(index, 1);
    },
     TagremoveTodo(command) {
       
       this.todoItems = [];

        for (var i = 0; i < localStorage.length; i++) {
              var tempTodoList = JSON.parse(localStorage.getItem(localStorage.key(i)));
              console.log(tempTodoList);

              if (command == 'nomal' && tempTodoList.picked == 'nomal')
                this.todoItems.push(tempTodoList);
              if (command == 'important' && tempTodoList.picked == 'important')
                this.todoItems.push(tempTodoList);
              if (command == 'emergency' && tempTodoList.picked == 'emergency')
                this.todoItems.push(tempTodoList);
              if (command == 'all')
                this.todoItems.push(tempTodoList);
        }
      this.todoItems.sort(function(a,b){return a['isComplete']>b['isComplete']});
    },
    sortTodo(sort){
      if(sort == 'date'){
        this.todoItems.sort(function(a,b){return a['date']>b['date']});
        this.todoItems.sort(function(a,b){return a['date']==''});
      }
      if(sort == 'bomb')
        this.todoItems.sort(function(a,b){return a['picked']>b['picked']});
    
      this.todoItems.sort(function(a,b){return a['isComplete']>b['isComplete']});
    },
    searchTodo(key) {
      this.search = key
      //console.log(this.search)
    },

    // Todo 상태를 변경시키기 위한 함수
    // {'title': v , 'date':v, 'context':v, 'isComplete':v}
      completeTodo(todoItem){
      todoItem.isComplete = !todoItem.isComplete;
      localStorage.setItem(todoItem.title, JSON.stringify(todoItem));    
    },

    // {'title': v , 'date':v, 'context':v, 'isComplete':v}
     editTodo(todoItem, index){
         localStorage.setItem(todoItem.title, JSON.stringify(todoItem));
       }
  },  
  created() {
      if (localStorage.length > 0) {
         for (var i = 0; i < localStorage.length; i++) {
        this.todoItems.push(JSON.parse(localStorage.getItem(localStorage.key(i))));
      }
      //페이지가 로드 될 때
      this.todoItems.sort(function(a,b){return a['isComplete']>b['isComplete']});
     }
  },
  components: {
    'TodoHeader': TodoHeader,    'TodoInput': TodoInput,
    'TodoList': TodoList,    'TodoFooter': TodoFooter,
    'TodoAlert':TodoAlert
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