<template>
	<button 
		class="btn"
		@click="$emit('currFloor',btnNumber)"
	>
	</button>
</template>

<script>
export default {
	name: 'vButton',
	props:['btnNumber','floorinQueue'],
}
</script>

<style>
	.btn {
		height: 30px;
		width: 30px;
		background-color: wheat;
		border: 1px solid;
		cursor: pointer;
	}
	.btn_blink {
		animation-name: buttonBlink;
		animation-iteration-count:infinite;
		animation-duration: 1s;
	}	
	@keyframes buttonBlink {
		from { background-color: wheat }
		to { background-color: rgba(255, 255, 255, 0.2) }
	}
</style>