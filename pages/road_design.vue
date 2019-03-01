<template lang="pug">
  .block-list
    .block-list(v-for="(sel_block,sel_block_id) in select_block_data" :key="`key-${sel_block_id}`")
      BlockSelect(@blockSelect="blockSelect" :block="sel_block" :list_id="sel_block.list_id")
    .list-add-button(@click="blockAdd")
      v-icon fas fa-plus
    Canvas(:blocks="select_block_data" ref="canvas")
</template>
<script>
import BlockSelect from '~/components/BlockSelect.vue'
import Canvas from '~/components/Canvas.vue'
import BlockData from '~/assets/json/block_data.json'

export default {
  components: {
    BlockSelect,
    Canvas
  },
  mounted(){
  },
  data(){
    return{
      biomes:[
        {id:"plains"},
        {id:"forest"},
        {id:"desert"},
        {id:"swamp"},
        {id:"tundra"}
      ],
      select_block_data:[
        {
          list_id: 0,
          id: "grass_path_top",
          name: "Grass Path",
          jp: "小道",
          per: 100
        }
      ],
      block_data: BlockData
    }
  },
  methods:{
    blockSelect(block,id){
      this.select_block_data[id] = block
      this.forcePercent(id)
    },
    blockAdd(){
      var list_id = this.select_block_data.length
      var block = {
        list_id: list_id,
        id: "grass_path_top",
        name: "Grass Path",
        jp: "小道",
        per: 0
      }
      this.select_block_data.push(block)
      this.forcePercent(list_id+1)
    },
    forcePercent(id){
      var total_percent = 0
      for(var i=0; i<this.select_block_data.length; i++){
        total_percent += this.select_block_data[i].per;
      }
      console.log(total_percent)
      if(total_percent!==100){
        console.log("not 100%")
        var diff = 100 - total_percent
        var one_diff = Math.floor(Math.abs(diff)/(this.select_block_data.length-1))
        console.log("one_diff:"+one_diff)
        for (let i = 0; i < this.select_block_data.length; i++) {
          if(this.select_block_data[i].list_id!==id){
            if(diff<0){
              this.select_block_data[i].per -= one_diff
            }else if(diff>0){
              this.select_block_data[i].per += one_diff
            }
          }
        }
        var totyu_total_percent = 0
        for(var i=0; i<this.select_block_data.length; i++){
          totyu_total_percent += this.select_block_data[i].per;
        }
        console.log("totyu:"+totyu_total_percent)

        var surplus = diff%(this.select_block_data.length-1)
        console.log(`surplus:${surplus}`)
        var count = 0;
        for (let i = 0; i < this.select_block_data.length; i++) {
          if(this.select_block_data[i].list_id!==id){
            if(surplus<0){
              this.select_block_data[i].per -= 1
              count++
            }else if(surplus>0){
              this.select_block_data[i].per += 1
              count++
            }
            if(Math.abs(Number(surplus) <= count)){
              break;
            }
          }
        }
      }
      var modified_total_percent = 0
      for(var i=0; i<this.select_block_data.length; i++){
        modified_total_percent += this.select_block_data[i].per;
      }
      console.log("modified:"+modified_total_percent)
      this.draw()
    },
    draw(){
      this.$refs.canvas.draw();
    }
  }
}
</script>
<style lang="scss">
.list-box {
  width: 200px;
}
</style>
