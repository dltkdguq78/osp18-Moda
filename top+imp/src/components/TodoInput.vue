<template>
	<section>
	<div class="inputBox shadow">
		<input type="text" v-model="newTodoItem" placeholder="Type what you have to do" v-on:keyup.enter="addModal=true">
		<span class="addContainer" v-on:click="addModal=true">
			<i class="addBtn fas fa-plus" aria-hidden="true"></i>
		</span>

		<modal v-if="showModal" @close="showModal = false" >
			<h3 slot="header">경고</h3>
			<span slot="footer" @click="showModal = false">할 일을 입력하세요.
				<i class="closeModalBtn fas fa-times" aria-hidden="true" @click="addModal = false"></i>
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
				<i class="closeModalBtn fas fa-check" aria-hidden="true" @click="addTodo"></i>
				<i class="closeModalBtn fas fa-times" aria-hidden="true" @click ="clearInput"></i>
			</span>
		</modal>
	</section>
</template>

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
				picked : ''
			}
		},
		methods: {
			addTodo() {
				if (this.newTodoItem !== "") {
					var title = this.newTodoItem && this.newTodoItem.trim();
					var context = this.newTodoContext && this.newTodoContext.trim();
					this.$emit('addTodo', title,context,this.newTodoDate,this.picked);
					this.clearInput();
				} else {
					this.showModal = !this.showModal;
				}
			},
			clearInput() {
				this.addModal = false;
				this.newTodoItem = '';
				this.newTodoDate = '';
				this.newTodoContext = '';
				this.picked='';
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
	.addBtn {
		color: white;
		vertical-align: middle;
	}
</style>
