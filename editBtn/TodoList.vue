<template>
	<section>

		<modal v-if="showModal">
			<h3 slot="header">수정</h3>
			<span slot = "input" @keyup.enter="editTodo">
				<input type="text" v-model="editTodoItem">
				<i class="closeModalBtn fas fa-check" aria-hidden="true" @click="editTodo"></i>
				<i class="closeModalBtn fas fa-times" aria-hidden="true" @click ="clearInput"></i>
			</span>
		</modal>

		<transition-group name="list" tag="ul">
			<li v-for="(todoItem, index) in propsdata" :key="todoItem" class="shadow">
				<i class="checkBtn fas fa-check" aria-hidden="true"></i>
				{{ todoItem }}

				<span class="editBtn" type="button" @click="save(todoItem,index)">
					<i class="far fa-edit" aria-hidden="true"></i>
				</span>
				<span class="removeBtn" type="button" @click="removeTodo(todoItem, index)">
					<i class="far fa-trash-alt" aria-hidden="true"></i>
				</span>
			</li>
		</transition-group>
	</section>
</template>

<script>
	import Modal from './common/Modal.vue'

	export default {
			data() {
				return {
					saveTodo:'',
					saveIndex:'',
					editTodoItem: '',
					showModal: false
				}
			},
		props: ['propsdata'],
		methods: {
			removeTodo(todoItem, index) {
				this.$emit('removeTodo', todoItem, index);
			},
			editTodo() {
				this.showModal = !this.showModal;
				if (this.editTodoItem !== "") {
					var value = this.editTodoItem && this.editTodoItem.trim();
					this.$emit('editTodo',this.saveTodo,value,this.saveIndex);
					}
					this.editTodoItem="";
				},
				save(todoItem,index){
				  this.showModal=!this.showModal;
					this.saveIndex=index;
					this.saveTodo=todoItem;
				},
				clearInput() {
					this.editTodoItem = '';
					this.showModal = !this.showModal;
				}
			},
			components: {
				Modal: Modal
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
		margin-left: 25px;
		color: #de4343;
	}
	.editBtn {
		margin-left: auto;
		color: #62acde;
	}

	.list-enter-active, .list-leave-active {
		transition: all 1s;
	}
	.list-enter, .list-leave-to {
		opacity: 0;
		transform: translateY(30px);
	}
</style>
