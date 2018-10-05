<template>
	<div class="clearAllContainer">
		<span class="clearAllBtn" @click="clearTodo">Clear All</span>
	</div>
</template>

<script>
	export default {
		methods: {
			clearTodo() {
				this.$emit('removeAll');
			},
			topFunction() { //top버튼 구현 수정 필요 
    			document.body.scrollTop = 0;
    			document.documentElement.scrollTop = 0;
			}
		}
	}
</script>

<style scoped>
	.clearAllContainer {
		width: 8.5rem;
		height: 50px;
		line-height: 50px;
		background-color: white;
		border-radius: 5px;
		margin: 0 auto;
	}
</style>
