<template lang="pug">
  .canvas(:class="{'perspective':perspective}")
    .flat-scenery(v-if="loading==false")
      img.back-image(:src="`../flat_scenery/flat_plains.png`" :class="{'view-image':view_image}")
    canvas#canvas(width="1200px" height="1200px")
    .loading(v-if="loading")
      .loading-wrapper
        v-icon.icon fas fa-spinner
        .loading-text Now loading ...
</template>
<script>
import BlockData from '~/assets/json/block_data.json'
export default {
  props:["blocks","road"],
  data(){
    return {
      canvas:{
        width:"1200",
        height:"1200"
      },
      image_size: 30,
      backimg_id:'grass_plains',
      image_file:[],
      block_data: BlockData,
      biomes:["plains","forest","desert","swamp","tundra"],
      biome:"plains",
      loading:true,
      perspective:false,
      view_image:false,
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

            for (let j = 0; j < this.biomes.length; j++) {
              image_file.push({
                id:`grass_${this.biomes[j]}`,
                img: new Image(),
                load: false
              })
              image_file[block_length+j].img.src = `../block/grass_${this.biomes[j]}.png?${new Date().getTime()}`
              image_file[block_length+j].img.onload = () => {
                image_file[block_length+j].load = true;
                if(j+1 >= this.biomes.length){
                  this.image_file = image_file;
                  this.loading = false
                  this.draw();
                }
              }
            }

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

      var temp_backimg = ''
      if(this.backimg_id=='grass_plains'){
        temp_backimg = `grass_${this.biome}`
      }else{
        temp_backimg = this.backimg_id
      }

      var backimg = this.image_file.find((file) => {
          return (file.id === temp_backimg);
      });
      for (let len = 0; len < back_len; len++) {
        for (let wid = 0; wid < back_wid+1; wid++) {
          var backx = (len*size);
          var backy = (wid*size) - (top%size);
          ctx.drawImage(backimg.img, backx, backy, size, size);
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
        var random_block = percent_blocks[random]
        if(random_block=='grass_plains'){
          return `grass_${this.biome}`
        }else{
          return random_block
        }
      }else{
        return "grass_path_top"
      }
    },
    settingUpdate(property,data){
      if(property=='block'){
        this.backimg_id = data.id
        this.draw();
      }else if(property=='road_wid'){
        this.road.wid = data
        this.draw();
      }else if(property=='road_leng'){
        this.road.leng = data
        this.draw();
      }else if(property=='biome'){
        this.biome = data
        this.draw();
      }else if(property=='perspective'){
        this.perspective = data
      }else if(property=='viewimage'){
        this.view_image = data
      }
    }
  }
}
</script>
<style lang="scss">
.canvas{
  position: relative;
  width: 1200px;
  height: 500px;
  background: #44662e;
  perspective: 800px;
  overflow: hidden;
  .flat-scenery {
    background: #8CB0F9;
    width: 100%;
    height: 100%;
    img.back-image {
      position: absolute;
      top: -200px;
      left: 0;
      width: 1200px;
      height: auto;
      opacity: 0;
      transition: 1s ease-in-out;
    }
    img.view-image{
      opacity: 1;
    }
  }
  canvas{
    position: absolute;
    margin: auto;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    width: 1200px;
    height: 1200px;
    transition: 1s ease-in-out;
    transform: rotateZ(0deg)rotateY(0deg)scale(1);
  }
  .loading{
    position: absolute;
    width: 100%;
    height: 100%;
    padding: 60px 0;
    .loading-wrapper {
      position: absolute;
      margin: auto;
      top: 0;
      bottom: 0;
      width: 100%;
      text-align: center;
      height: min-content;
      .icon {
        font-size: 30px;
        animation: loading 2s linear 0s infinite;
      }
      .loading-text {
        font-size: 20px;
      }
    }
  }
}
.perspective {
  canvas{
    transform: rotateZ(90deg)rotateY(-80deg)scale(.865);
  }
}

@keyframes loading {
  0% {transform: rotateZ(0deg)}
  100% {transform: rotateZ(360deg)}
}
</style>
