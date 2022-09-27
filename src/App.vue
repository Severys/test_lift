<template>
  <div id="app">
    <otherfloors 
      v-for="(floor,index) in floors"
      :key="index"
      :floorNumber="floor"
      :floorinQueue="queue"
      @currFloor="el => addtoQueue(el)"
    />
    <div class="basement">
      <div 
        class="lift"
        ref="lift"
        :class="{blink: lift.isBlink}"
      >
        <div 
          v-if="lift.nextFloor !== ''" 
          class="lift_direction"
        >
          {{this.lift.nextFloor}}
          {{this.lift.direction === 'down'? '&#8595;' : '&#8593;'}}
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Otherfloors from './components/otherfloors.vue'

export default {
  name: 'App',
  components: {
    Otherfloors,
},
  data(){
    return {
      numberofFloors: 5,
      floors: [],
      lift: {
              curFloor: 1,
              nextFloor: '',
              isBlink: false
            },
      queue: [],
      readytoGo: Boolean
    }
  },
  methods:{
    createHouse(){
      for(var f = this.numberofFloors; f !== 0; f-- ){
        this.floors.push(f)
      }
    },
    addtoQueue(el){
      if(this.queue.indexOf(el) === -1 && this.lift.curFloor !== el) {
        if(this.queue.length === 0) this.readytoGo = true
        this.queue.push(el)
      }
    },
    movelift(floor){
      this.readytoGo = false
      this.lift.nextFloor = floor
      if(this.lift.curFloor - floor < 1) this.lift.direction = 'up'
      else this.lift.direction = 'down'
      let curTrans = Math.abs(this.lift.curFloor - floor) * 1000
      let curTop = (floor * -150) - floor*2 +1
      this.$refs.lift.style.transition = `${curTrans}ms`
      this.$refs.lift.style.top = `${curTop}px`
      this.relaxlift(curTrans,floor)
    },
    relaxlift(trans, curFloor){
      setTimeout(() => {
        this.lift.isBlink = true
        this.lift.curFloor = curFloor
        this.lift.nextFloor = ''
        this.queue.shift()
      }, trans);
      setTimeout(()=>{
        this.lift.isBlink = false
        if(this.queue.length === 0) this.readytoGo = Boolean
        else this.readytoGo = true
      }, trans + 3000)
    },
  },
  mounted(){
    this.createHouse()
  },
  watch:{
    readytoGo: function (){
      if(this.queue.length !== 0 && this.readytoGo === true) this.movelift(this.queue[0])
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
.basement{
  display: flex;
  gap: 12px;
}
.lift{
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  color: white;
  font-size: 32px;
  width: 150px;
  height: 150px;
  background-color: red;
  position: relative;
  top: -151px;
  left: 2px;
}
.lift_direction{
  background-color: brown;
  border-radius: 10px;
  padding: 15px;
}
.blink {
  animation-name: blinker;
  animation-iteration-count: 3;
  animation-duration: 1s;
}
@keyframes blinker {
  from { opacity: 1.0; }
  to { opacity: 0.0; }
}
</style>
