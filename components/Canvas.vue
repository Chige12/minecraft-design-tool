<template lang="pug">
  .canvas
    canvas#canvas(width="1200px" height="400px")
</template>
<script>
import BlockData from '~/assets/json/block_data.json'
export default {
  props:["blocks"],
  data(){
    return {
      canvas:{
        width:"1200",
        height:"400"
      },
      image_size: 30,
      road:{
        wid: 3,
        leng: 24
      },
      image_file:[],
      block_data: BlockData
    }
  },
  mounted(){
    this.$nextTick(() => {
      this.imageLoad()
    })
  },
  methods: {
    imageLoad(){
      var image_file = []
      var block_length = this.block_data.length;
      for(let i=0; i < block_length; i++){
        image_file.push({
          id: this.block_data[i].id,
          img: new Image(),
          load: false
        })
        image_file[i].img.src = `../block/${this.block_data[i].id}.png?${new Date().getTime()}`
        image_file[i].img.onload = () => {
          image_file[i].load = true;
          if(i+1 >= block_length){
            console.log(`load success!  ${i+1}/${block_length}`)
            this.image_file = image_file;
            // 画像読み込み後に描画
            this.draw();
          }else{
            console.log(`now loading... ${i+1}/${block_length}`)
          }
        }
      }
    },
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

      var grass = this.image_file.find((file) => {
          return (file.id === 'grass_plains');
      });
      for (let len = 0; len < back_len; len++) {
        for (let wid = 0; wid < back_wid+1; wid++) {
          var backx = (len*size);
          var backy = (wid*size) - (top%size);
          ctx.drawImage(grass.img, backx, backy, size, size);
        }
      }
      if(this.blocks.length!==0){
        var any_block
        for (let len = 0; len < this.road.leng; len++) {
          for (let wid = 0; wid < this.road.wid; wid++) {
            var any_block_id = this.RandomBlock()
            any_block = this.image_file.find((img) => {
                return (img.id === any_block_id);
            });
            x = left + (len * size);
            y = top + (wid * size);
            ctx.drawImage(any_block.img, x, y, size, size);
          }
        }
      }
    },
    ImageSize(){
      var width = (this.canvas.width) / (this.road.leng)
      var height = (this.canvas.height) / (this.road.wid)
      return Math.min(width,height);
    },
    StartPoint(size){
      var road_width = size * this.road.wid
      var road_length = size * this.road.leng
      var top = (this.canvas.height / 2) - (road_width / 2)
      var left = (this.canvas.width / 2) - (road_length / 2)
      return {top:top,left:left}
    },
    RandomBlock(){
      var percent_blocks = []
      for (let i = 0; i < this.blocks.length; i++) {
        for (let j = 0; j < this.blocks[i].per; j++) {
          percent_blocks.push(this.blocks[i].id)
        }
      }
      if(percent_blocks.length){
        var random = Math.floor( Math.random()*100 );//0 ~ 99
        return percent_blocks[random]
      }else{
        return "grass_path_top"
      }
    }
  }
}
</script>
<style lang="scss">
canvas{
  position: absolute;
  top: 0;
  left: 0;
  width: 1200px;
  height: 400px;
}
</style>
