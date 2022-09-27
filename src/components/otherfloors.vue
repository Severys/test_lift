<template>
	<div class="floor">
		<div class="floor_shaft"></div>
		<div class="floor_plate">
			<div class="floor_plate_number">
				{{floorNumber}} этаж
			</div>
			<v-button
				:class="{btn_blink: inQueue}"
				:btnNumber="floorNumber"
				:floorinQueue="floorinQueue"
				@currFloor="el => $emit('currFloor', el)"
			/>
		</div>
	</div>
</template>

<script>
import vButton from './v-button.vue'

export default {
	name: 'otherFloors',
	components: { vButton },
	props:['floorNumber', 'shaftsCount','floorinQueue'],
	data(){
		return {
			inQueue: false
		}
	},
	watch:{
		floorinQueue: function (){
			if(this.floorinQueue.indexOf(this.floorNumber) >= 0) this.inQueue = true
			else this.inQueue = false
		}
	}
}
</script>

<style>
	.floor{
		border: 1px solid;
		height: 150px;
		display: flex;
		gap: 10px;
		align-items: center;
	}
	.floor_shaft{
		width: 150px;
		height: 100%;
		border-inline: 1px solid;
	}
	.floor_plate {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		gap: 10px;
		height: 100%;
	}
</style>