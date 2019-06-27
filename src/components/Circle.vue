
<template>
    <div class="circle">
        <canvas-circle 
        v-for="(item,index) in cicleList" 
        :key="index" 
        :percent="item.ratio"
        :unit="'分'"
        :id="index"
        :width=200
        :height=200
        :ringR=17
        :speed=0.4>
        </canvas-circle>
    </div>
</template>
<script>
import canvasCircle from './canvasCircle'
export default {
    name: 'CircleAni',
    components: {
    canvasCircle
  },
  data () {
    return {
      cicleList: [{
        ratio: 35
      },{
        ratio: 65
      },{
        ratio: 85
      }]
    }
  }
}
</script>