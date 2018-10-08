<template>
	<header>
		<h1 @click="onScroll">To Do It!</h1>
			<button class = "topBtn" v-on:click="onScroll">
				<i class="fas fa-angle-double-up" aria-hidden="true"></i>
			</button>
	</header>
</template>

<script>
	export default {
	}
</script>

<script>
	export default {
		  methods: {
			onScroll() { //top버튼 구현 수정 필요 
    			document.body.scrollTop = 0;
    			document.documentElement.scrollTop = 0;
			},
 		    handleScroll (event) {
				if (document.body.scrollTop > 10 || document.documentElement.scrollTop > 10) {
					document.getElementsByClassName("topBtn")[0].style.display = "block";
				} else {
					document.getElementsByClassName("topBtn")[0].style.display = "none";
				}
   			 }
		},
		created () {
			window.addEventListener('scroll', this.handleScroll);
		},
		destroyed () {
			window.removeEventListener('scroll', this.handleScroll);
		}
	}
</script>

<style scoped>
	h1 {
		color: #2F3B52;
		font-weight: 900;
		margin: 2.5rem 0 1.5rem;
	}
	.topBtn {
		display: none;
		position: fixed;
		width: 3rem;
		bottom: 20px;
		right: 20px;
		z-index: 99;
		font-size: 18px;
		border: none;
		outline: none;
		background: linear-gradient(to left, rgb(184, 75, 67), rgb(206, 98, 125));
		color: white;
		cursor: pointer;
		padding: 15px;
		border-radius: 5px;
	}
</style>
