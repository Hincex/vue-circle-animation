<template>
  <div class="canvas">
    <canvas :id="id" :width="width" :height="height"></canvas>
  </div>
</template>

<script>
export default {
  data () {
    return {
      //canvas实例
      context: '',
      cirX: '',
      cirY: '',
      rad: '',
      n: 1,
      r: this.width/2,
      fontSize: this.width * (40 / 136),
    }
  },
  props:{
    //传入百分比
    percent: {
      default: 0,
      type: Number
    },
    //显示单位
    unit: {
      default: '%',
      type: String
    },
    //传入id
    id: {
      default: 0,
      type: Number
    },
    //圆环宽度
    width: {
      default: 136,
      type: Number
    },
    //圆环高度
    height: {
      default: 136,
      type: Number
    },
    //外圆环粗细
    ringR: {
      default: 17,
      type:Number
    },
    //动画速度
    speed: {
      default:0.4,
      type: Number
    }
  },
  //元素挂载后开始绘制并且获取
  mounted () {
    this.canvas = document.getElementById(this.id);
    this.context = this.canvas.getContext("2d");
    //所有圆的圆心坐标
    this.cirX = this.width / 2;
    this.cirY = this.height / 2;
    this.rad = Math.PI * 2 / 100;
    this.DreamLoading()
  },
  methods:{
    //绘制最外层细圈
    drawRing(){
        this.context.save();         //save和restore保证样式属性只运用于该段canvas元素
        this.context.beginPath();    //开始路径
        this.context.strokeStyle = "#eceef3";       //设置边线的颜色
        this.context.lineWidth = this.ringR;
        this.context.arc(this.cirX, this.cirY, this.r-this.context.lineWidth/2, 0, Math.PI * 2, false);      //画一个圆的路径
        this.context.stroke();       //绘制边线
        this.context.closePath();
        this.context.restore();
    },
    //绘制中心白圆
    drawCenterWhite(){
        this.context.save();         //save和restore可以保证样式属性只运用于该段canvas元素
        this.context.shadowColor = "#c0c0c0";
        this.context.shadowBlur = 8;
        this.context.shadowOffsetX = 0;
        this.context.shadowOffsetY = 0;
        this.context.beginPath();
        this.context.arc(this.cirX, this.cirY, this.r - this.ringR, 0, Math.PI * 2, false);      //画一个圆的路径
        this.context.fillStyle = "#fff";       //设置填充颜色  
        this.context.fill();
        this.context.closePath();
        this.context.restore();
    },
    //绘制渐变外圈
    drawGradient(n){
        let angle = 2 * Math.PI + Math.PI/2 - this.rad * n;
        let xPos = Math.cos(angle) * (this.r-this.ringR/2) + this.width/2;
        let yPos = -Math.sin(angle) * (this.r-this.ringR/2) + this.height/2;
        this.context.save();
        let grd = this.context.createLinearGradient(this.width/2,0,0,this.height);
        grd.addColorStop(0, "#00d3a4");
        grd.addColorStop(0.5, "#14c7ca");
        grd.addColorStop(1, "#14c7ca");
        this.context.strokeStyle=grd;
        this.context.lineWidth = this.ringR;
        this.context.lineCap = 'round';
        this.context.beginPath();
        this.context.arc(this.cirX, this.cirY, this.r-this.context.lineWidth/2, -Math.PI/2,-Math.PI/2 + this.rad*n, false);
        this.context.stroke();
        this.context.closePath();
        this.context.restore();
    },
    //绘制文本
    drawText(n){
        this.context.save();
        this.context.font =  this.fontSize + 'px' + ' Arial';
        this.context.fillStyle = "#000"; //字体颜色
        this.context.fillText(n.toFixed(0),this.cirX - this.fontSize / 1.5,this.cirY + this.fontSize/2.5); //绘制数字
        this.context.font =  this.fontSize * 0.5 + 'px' + ' Arial';
        this.context.fillText(this.unit,this.cirX + this.fontSize * 0.5,this.cirY + this.fontSize/3); //绘制单位
        this.context.restore();
    },
    //绘制渐变外圈上的小白圆
    drawOuterWhite(n){
        let angle = 2 * Math.PI + Math.PI/2 - this.rad * n;
        let xPos = Math.cos(angle) * (this.r-this.ringR/2) + this.width/2;
        let yPos = -Math.sin(angle) * (this.r-this.ringR/2) + this.height/2;
        this.context.save();
        this.context.beginPath();
        this.context.arc(xPos,yPos,this.ringR/3,0,Math.PI * 2,false);
        this.context.fillStyle = '#fff';
        this.context.fill();
        this.context.closePath();
        this.context.restore();
    },  
    DreamLoading(){
        //清除所有,重新绘制
        this.context.clearRect(0,0,this.width,this.height);
        this.drawRing();
        this.drawCenterWhite();
        this.drawGradient(this.n);
        this.drawText(this.n);
        this.drawOuterWhite(this.n);
        //数字变化
        if(this.n <= this.percent){
            this.n = this.n + this.speed;
        }else {
          return this.n = 0;
        }
        //实现逐帧动画效果
        requestAnimationFrame(this.DreamLoading);
    }
  }
}
</script>
<style scoped>
</style>
