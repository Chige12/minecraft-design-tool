<template lang="pug">
  canvas#canvas(width="800px" height="400px")
</template>
<script>
export default {
  props:["blocks"],
  data(){
    return {
      canvas:{
        width:"800",
        height:"400"
      },
      image_size: 30,
      road:{
        wid: 3,
        leng: 20
      }
    }
  },
  mounted(){
    this.$nextTick(() => {
      this.draw();
    })
  },
  methods: {
    draw(){
      var canvas = document.getElementById('canvas');
      if ( ! canvas || ! canvas.getContext ) { return false; }
      var ctx = canvas.getContext('2d');

      /* スムージングを無効化 */
        ctx.imageSmoothingEnabled = false;
        ctx.mozImageSmoothingEnabled = false;
        ctx.webkitImageSmoothingEnabled = false;
        ctx.msImageSmoothingEnabled = false;
      
      var size = this.ImageSize()
      var top = this.StartPoint(size).top
      var left = this.StartPoint(size).left

      var angle = 0;
      var rad = 0;
      var x,y = 0

      var back_len = this.canvas.width / size
      var back_wid = this.canvas.height / size
      for (let len = 0; len < back_len; len++) {
        for (let wid = 0; wid < back_wid; wid++) {
          var backimg = new Image();
          backimg.src = `../block/grass_plains.png?${new Date().getTime()}`
          backimg.onload = function() {
            var backx = (len*size);
            var backy = (wid*size);
            console.log(backx)
            ctx.drawImage(backimg, backx, backy, size, size);
          }
        }
      }

      for (let len = 0; len < this.road.leng; len++) {
        for (let wid = 0; wid < this.road.wid; wid++) {
          var img = new Image();
          img.src = `../block/${this.blocks[0].id}.png?${new Date().getTime()}`
          // 画像読み込み後に描画
          img.onload = function() {
            x = left + (len * size);
            y = top + (wid * size);
            ctx.drawImage(img, x, y, size, size);
          }
        }
      }
    },
    ImageSize(){
      var width = (this.canvas.width) / (this.road.leng+2)
      var height = (this.canvas.height) / (this.road.wid+2)
      return Math.min(width,height);
    },
    StartPoint(size){
      var road_width = size * this.road.wid
      var road_length = size * this.road.leng
      var top = (this.canvas.height / 2) - (road_width / 2)
      var left = (this.canvas.width / 2) - (road_length / 2)
      return {top:top,left:left}
    },
    GetRandomNumber(num) {
      return Math.floor( Math.random() * num );//0 ~ (num-1)
    },
    RandomBlock(){
      //% random(0~2)
    }
  }
}
</script>
<style lang="scss">
#canvas {
  width: 800px;
  height: 400px;
}
</style>
