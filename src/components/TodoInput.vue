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
					<input type="radio" id="nomal" value="nomal" style = "width:15px;" v-model="picked">
					<label for="nomal">보통</label>
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
			
		<div class ="tagimportBtn">
                    <input type="radio" id="All" value="All" @click="TagremoveTodo('all')" style = "width:15px;" v-model="tagpicked">
                    <label for="All">전체</label>
                    <input type="radio" id="nomal" value="nomal" @click="TagremoveTodo('nomal')" style = "width:15px;" v-model="tagpicked">
                    <label for="nomal">보통</label>
                    <input type="radio" id="important" value="important" @click="TagremoveTodo('important')"  style = "width:15px;" v-model="tagpicked">
                    <label for="important">중요</label>
                    <input type="radio" id="emergency" value="emergency" @click="TagremoveTodo('emergency')"  style = "width:15px;" v-model="tagpicked">
                    <label for="emergency">긴급 </label>
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
				picked : 'nomal',
				tagpicked : ''
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
				this.picked='nomal';
			},
			TagremoveTodo(command){
            	this.$emit('TagremoveTodo', command);
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
	input:focus {
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
	}
	.inputBox input {
		border-style: none;
		font-size: 0.9rem;
	}
	.addContainer {
		float: right;
		background: linear-gradient(to right, #6478FB, #8763FB);
		display: inline-block;
		width: 3rem;
		border-radius: 0 5px 5px 0;
	}
	.searchContainer {
		float: left;
		background: linear-gradient(to left, rgb(219, 163, 43), rgb(219, 119, 52));
		display: inline-block;
		width: 3rem;
		border-radius: 5px 0 0 5px;
	}
	.Btn {
		color: white;
		vertical-align: middle;
	}

	.tagimportBtn{
		padding : 12px;
		width: 100%;
	}
</style>
