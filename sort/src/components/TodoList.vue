<template>
    <section>
        <modal v-if="editModal">
            <h3 slot="header">수정</h3>
            <span slot = "input" @keyup.enter="editTodo">
                <label>할 일</label>
                <p style="color:#000000">{{saveTodo.title}}</p>
                <label>마 감 날 짜</label>
                <input type = "date" v-model="editDate"><!--editdate에 달력이들어가는데 model로 데이타 연동됨 -->
                <label>내 용</label>
                <textarea id="subject" v-model="editContext" style="height:100px;"></textarea><!--editContext에 내용이들어가는데 model로 데이타 연동됨 -->
                <label>중 요 도</label>
                <div class ="importBtn">
                    <input type="radio" id="nomal" value="nomal" style = "width:15px;" v-model="editimportant">
                    <label for="nomal">보통</label>
                    <input type="radio" id="important" value="important"  style = "width:15px;" v-model="editimportant">
                    <label for="important">중요</label>
                    <input type="radio" id="emergency " value="emergency"  style = "width:15px;" v-model="editimportant">
                    <label for="emergency ">긴급 </label>
                </div>
            </span>       
            <span slot = "footer">   
                <i class="ModalBtn fas fa-check" aria-hidden="true" @click="editTodo"></i> <!--수정창 떳을때 [checkbtn] , times -->
                <i class="ModalBtn fas fa-times" aria-hidden="true" @click ="clearInput"></i><!--수정창 떳을때 checkbtn , [times] -->
            </span>
        </modal>
      <!--
         for todoItem,Index in TodoItems {
             V = {'title': v , 'date':v, 'context':v, 'isComplete':v}
             V.title
             V.isComplete
         } -->
        <transition-group name="list" tag="ul">   
            <div v-for="(todoItem, index) in propsdata" :key="todoItem.title">
                <div v-if="searchTodo(todoItem.title,search)"><!--search 추가 -->
                <li class="shadow" :title = todoItem.context sytle="height: 50px;">
                    <div v-if="todoItem.isComplete == false"  @click="completeTodo(todoItem)">
                        <div v-if="todoItem.picked=='important'">
                            <i class="checkBtn fas fa-check" style = "color:#FFBF00" aria-hidden="true"></i>
                            {{ todoItem.title }}
                        </div>
                        <div v-else-if="todoItem.picked == 'emergency'">
                            <i class="checkBtn fas fa-check" style = "color:#FF0000" aria-hidden="true"></i><!--젤왼쪽 [checkbtn] -->
                            {{ todoItem.title }}
                        </div>
                        <div v-else>
                            <i class="checkBtn fas fa-check" aria-hidden="true"></i><!--젤왼쪽 [checkbtn] -->
                                {{ todoItem.title }}
                        </div>
                    </div>
                    <div v-else>
                        <i class="checkBtn fas fa-check" style = "color:#A4A4A4" 
                            aria-hidden="true" @click="completeTodo(todoItem)"></i><!--젤왼쪽 [checkbtn] -->
                            <del>{{ todoItem.title }}</del> <!-- del태그로 완료된 태그 밑줄그어짐 -->
                    </div>
                    <span class="moreInfo" type="button" @click="more(todoItem)">
				        <i class="fas fa-angle-down" aria-hidden="true"></i>
			        </span>
                    <span class="editBtn" type="button" @click="save(todoItem,index)"><!--[editbnt] , 휴지통 -->
                        <i class="far fa-edit" aria-hidden="true" @click="editModal =! editModal"></i>
                    </span>
                    <span class="removeBtn" type="button" @click="removeTodo(todoItem, index)"><!--editbnt , [휴지통] -->
                        <i class="far fa-trash-alt" aria-hidden="true"></i>
                    </span>
                 </li>
                <li v-show="todoItem.moreInfo">
                    <span v-if="todoItem.context!==''" style="padding-left:10px">{{todoItem.context}}</span>
                    <span v-if="todoItem.date!==''" style="padding:5px; margin-left : auto"> 마감 : {{todoItem.date}} </span>
                </li>
                </div>
            </div>
        </transition-group>
    </section>
</template>

<script>
   import Modal from './common/Modal.vue'

   export default {
         data() {
            return {
               saveTodo:'', //수정버튼누를때 데이터를 넘겨줌으로써 취소버튼을 눌러도 데이터가 안날라감
               saveIndex:'',// 위와동일
               editContext:'',  //내용연동
               editDate:'', //달력연동
               editimportant:'',
               editModal: false,
               moreInfo: false
            }
         },
      props: ['propsdata','search','sort'],
      methods: {
         removeTodo(todoItem, index) {
            this.$emit('removeTodo', todoItem, index);
         },
         editTodo() {
            this.saveTodo.context = this.editContext && this.editContext.trim();
            if(this.saveTodo.date!==this.editDate)
               this.saveTodo.date = this.editDate;
            this.saveTodo.moreInfo = false;
            this.saveTodo.picked = this.editimportant;
            this.$emit('editTodo',this.saveTodo,this.saveIndex);
            this.clearInput();
            },
         save(todoItem,index){
            this.saveIndex=index; //index번째 
            this.saveTodo=todoItem; // 컨택스트, 데이트 등 통째로담긴 todoItem 을 줌
            this.editContext=todoItem.context;
            this.editDate=todoItem.date;
            this.editimportant=todoItem.picked;
            },
         completeTodo(todoItem, index){
            this.$emit('completeTodo', todoItem, index);
            },
         searchTodo(key,search){     
            return (key.indexOf(search)>-1 || search ==='')
         },
         more(todoItem){
             if(todoItem.context !== ''||todoItem.date !== '')
                todoItem.moreInfo=!todoItem.moreInfo
         },
         clearInput() {
            this.editContext="";
            this.editDate="";
            this.editModal = false;
            this.editimportant='';
            }
         },
         components: {
            Modal: Modal,
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
      line-height: 50px;
      margin: 0.5rem 0;
      padding: 0 0.9rem;
      background: white;
      border-radius: 5px;
   }

   input[type=text],[type=date], textarea {
      width: 100%; /* Full width */
      padding: 12px; /* Some padding */  
      border: 1px solid #ccc; /* Gray border */
      border-radius: 4px; /* Rounded borders */
      box-sizing: border-box; /* Make sure that padding and width stays in place */
      margin-top: 6px; /* Add a top margin */
      margin-bottom: 16px; /* Bottom margin */
      resize: vertical; /* Allow the user to vertically resize the textarea (not horizontally) */
      resize:none;
   }
   .importBtn{
       width: 100%;
       color: #000000;
       padding: 12px;
   }

   .content {
       padding: 0 18px;
       overflow: hidden;
       background-color: #f1f1f1;
   }

   .checkBtn {
      line-height: 45px;
      color: #62acde;
      margin-right: 5px;
   }

   .moreInfo {
	  margin-left: auto;
	}
   
   .editBtn {
      margin-left: 25px;
      color: #62acde;
   }
   .removeBtn {
      margin-left: 25px;
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