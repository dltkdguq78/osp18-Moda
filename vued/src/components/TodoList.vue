<template>
  <section>
    <transition-group name="list" tag="ul">
      <li v-for="(todoItem, index) in propsdata" :key="todoItem.context" class="shadow">
        <span class="checkBtn" type="button" @click="completeTodo(todoItem.context, index)">
          <i class="fas fa-check" aria-hidden="true"></i>
        </span>
        
        <!-- div에 v-if 조건절을 추가하여 todoItme의 isComplete 키 값이 false 일 경우와 true 일 경우에 적절한 html 문법을 삽입한다 -->
        <div v-if="todoItem.isComplete == false">
        {{ todoItem.context }}
        </div>
        <div v-else>
        <del>{{ todoItem.context }}</del>
        </div>

        <span class="removeBtn" type="button" @click="removeTodo(todoItem.context, index)">
          <i class="far fa-trash-alt" aria-hidden="true"></i>
        </span>
      </li>
    </transition-group>
  </section>
</template>

<script>
export default {
  props: ['propsdata'],
  methods: {
    removeTodo(todoItem, index) {
      this.$emit('removeTodo', todoItem, index);
    },
    completeTodo(todoItem, index){
      this.$emit('completeTodo', todoItem, index);
    }

  }
}
</script>

<style scoped>
  ul {
    list-style-type: none;
    padding-left: 0px;
    margin-top: 0;
    text-align: left;
  }
  li {
    display: flex;
    min-height: 50px;
    height: 50px;
    line-height: 50px;
    margin: 0.5rem 0;
    padding: 0 0.9rem;
    background: white;
    border-radius: 5px;
  }
  .checkBtn {
    line-height: 45px;
    color: #62acde;
    margin-right: 5px;
  }
  
  .removeBtn {
    margin-left: auto;
    color: #de4343;
  }

  .list-enter-active, .list-leave-active {
    transition: all 1s;
  }
  .list-enter, .list-leave-to {
    opacity: 0;
    transform: translateY(30px);
  }
</style>
