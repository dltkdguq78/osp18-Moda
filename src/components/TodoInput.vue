<template>
	<section>
	<div class="inputBox shadow">
		<span class="searchContainer" v-on:click="searchTodo">
			<i class="Btn fas fa-search" aria-hidden="true"></i>
		</span>
		<input type="text" v-model="newTodoItem" placeholder="Type what you have to do" 
			v-on:keyup.enter="check" v-on:keyup="searchTodo"> <!-- v-on:keyup="searchTodo" -->
		<span class="addContainer" v-on:click="check">
			<i class="Btn fas fa-plus" aria-hidden="true"></i>
		</span>

		<modal v-if="showModal" @keyup="showModal = false" >
			<h3 slot="header">경고</h3>
			<span slot="context">
				할 일을 입력하세요.</span>
			<span slot="footer" @click="showModal = false">
				<i class="ModalBtn fas fa-times" aria-hidden="true" @click="addModal = false"></i>
			</span>
		</modal>
	</div>

	<modal v-if="addModal">
		<h3 slot="header">추가 내용 설정</h3>
		<span slot = "input" @keyup.enter="addTodo">
			<label>마 감 날 짜</label>
			<input type = "date" v-model="newTodoDate">
			<label>설 명</label>
			<textarea v-model="newTodoContext" placeholder = "내용을 입력하세요" 
				style="height:100px;"></textarea>
			<label>중 요 도</label>
			<div class ="importBtn">
				<input type="radio" id="normal" value="normal" style = "width:15px;" v-model="picked">
				<label for="normal">보통</label>
				<input type="radio" id="important" value="important"  style = "width:15px;" v-model="picked">
				<label for="important">중요</label>
				<input type="radio" id="emergency " value="emergency"  style = "width:15px;" v-model="picked">
				<label for="emergency ">긴급 </label>
			</div>				
			</span>
		<span slot = "footer">	
			<i class="ModalBtn fas fa-check" aria-hidden="true" @click="addTodo"></i>
			<i class="ModalBtn fas fa-times" aria-hidden="true" @click ="clearInput"></i>
		</span>
	</modal>
			
	<div class = "btnsdiv">
		<button v-for="pickbtn in buttons.pick" :key="pickbtn.title" class = "Btns" :class="pickbtn.id" 
		:title="pickbtn.title" v-on:click="TagremoveTodo(pickbtn.id)" v-text="pickbtn.title"></button>
		<div class="dropdown">
   			<button class="dropbtn">정렬 
      			<i class="fa fa-caret-down"></i>
    		</button>
			<div class="dropdown-content">
				<button	v-for="btn in buttons.sort" :key="btn.class" :title="btn.title" 
					class = "Btns" :class="btn.class" v-on:click="sortTodo(btn.class)">
					<i class = "fas" :class="'fa-'+btn.class"></i>
				</button>
			</div>
		</div>
    </div>
	</section>
</template>
<script src="http://ajax.googleapis.com/ajax/libs/jquery/1.10.2/jquery.min.js"></script>
<script>
	import Modal from './common/Modal.vue'

	export default {
		data() {
			return {
				newTodoItem: '',
				newTodoDate: '',
				newTodoContext:'',
				showModal: false,
				addModal: false,
				picked : 'normal',
				tagpicked : 'all',
				sortType:'',
				buttons:{
					'pick':[
						{'id' : 'all', 'title':'전체'},
						{'id' : 'normal', 'title':'보통'},
						{'id' : 'important', 'title':'중요'},
						{'id' : 'emergency', 'title':'긴급'}
					],
					'sort':[
						{'class':'sort-numeric-down', 'title':'날짜순으로 정렬'},
						{'class':'bomb', 'title':'중요도순으로 정렬'},
					]
				}
			}
		},
		methods: {
			addTodo() {
				if (this.newTodoItem !== "") {
					var title = this.newTodoItem && this.newTodoItem.trim();
					var context = this.newTodoContext && this.newTodoContext.trim();
					this.$emit('addTodo', title,context,this.newTodoDate,this.picked);
					this.clearInput();
					this.searchTodo()
				} else {
					this.showModal = !this.showModal;
				}
			},
			check(){
				var title = this.newTodoItem && this.newTodoItem.trim();
				if (title !== "")
					this.addModal=true;
				else {
					this.clearInput();
					this.showModal=true;
					}
			},
			clearInput() {
				this.addModal = false;
				this.newTodoItem = '';
				this.newTodoDate = '';
				this.newTodoContext = '';
				this.picked='normal';
			},
			TagremoveTodo(command){
				this.tagpicked = command;
            	this.$emit('TagremoveTodo', command);
			},
			sortTodo(type){
				//:class = "sortType == btn.class?'selected':''"
				if(type == 'sort-numeric-down')
					this.$emit('sortTodo', 'date');		
				else if(type == 'bomb')
					this.$emit('sortTodo', 'bomb');	
			},
			searchTodo(){
				var title = this.newTodoItem && this.newTodoItem.trim();
				this.$emit('searchTodo', title);
			}
		},
		components: {
			Modal: Modal
		}
	}
</script>

<style scoped>
	input:focus, .Btns:focus {
		outline: none;
	}

	input[type=date], textarea {
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

	.inputBox {
		background: white;
		height: 50px;
		line-height: 50px;
		border-radius: 5px;
		margin : auto;
		margin-bottom: 10px;
		width: 70%;
	}
	.inputBox input {
		border-style: none;
		font-size: 0.9rem;
	}
	.addContainer {
		float: right;
		background: linear-gradient(to right, #48c6ef, #6f86d6);
		display: inline-block;
		width: 3rem;
		border-radius: 0 5px 5px 0;
	}
	.searchContainer {
		float: left;
		background: linear-gradient(to right, #ff9a9e, #fad0c4);
		display: inline-block;
		width: 3rem;
		border-radius: 5px 0 0 5px;
	}
	.Btn{
		color: azure;
	}
	.btnsdiv{
		font-weight: bold;
		width: 100%;
		background: linear-gradient(to right, #ff9a9e, #6f86d6);
	}
	.Btns {
		border: none;
		background-color: inherit;
		padding:15px 28px;
		font-size: 16px;
		cursor: pointer;
		color: white;
	}
	.all:active, .all:hover{
      color: black;
	  background-color: white;
	}
	.normal:active, .normal:hover{
      color: #62acde;
	  background-color: white;
	}
	.important:active, .important:hover{
      color: #FFBF00;		
	  background-color: white; 
	}
	.emergency:active, .emergency:hover{	
		background-color: white;
    	color: #FF0000; 
	}
	.dropdown {
		float: right;
		overflow: hidden;
	}

	.dropdown .dropbtn {
		cursor: pointer;
		font-size: 16px;
		border: none;
		color: white;
		background-color: inherit;
		padding: 15px 28px;
	}

	.dropdown:hover .dropbtn {
		background-color: white;
		color:#6f86d6;
	}
	.dropdown-content {
		display: none;
		position: absolute;
		background-color: #f9f9f9;
		box-shadow: 0px 8px 16px 0px rgba(0,0,0,0.2);
		width: 103px;
		z-index: 1;
	}

	.dropdown-content button {
		color: black;
		display: block;
		width: 100%;
		text-align: center;
	}

	.dropdown-content button:hover {
		background-color: #6f86d6;
		color: white
	}

	.dropdown:hover .dropdown-content {
		display: block;
	}
</style>
