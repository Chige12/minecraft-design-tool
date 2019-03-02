<template lang="pug">
.road_design
  Canvas(:blocks="select_block_data" ref="canvas").canvas-parent
  .block-lists
    .block-list(v-for="(sel_block,sel_block_id) in select_block_data" :key="`key-${sel_block_id}`")
      BlockSelect(@blockSelect="blockSelect" @blockDelete="blockDelete" :block="sel_block" :disabled="disabled")
    .button.list-add-button(@click="blockAdd()")
      v-icon.icon fas fa-plus
      .text ブロックを追加
    .button.reload-Button(@click="draw()")
      v-icon.icon fas fa-sync-alt
      .text リロード
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
      block_data: BlockData,
      disabled: true
    }
  },
  methods:{
    blockSelect(block){
      this.select_block_data[block.list_id] = block
      this.forcePercent(block.list_id)
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
      if(this.select_block_data.length > 0){
        this.disabled=false
      }
      console.log(list_id)
      this.forcePercent(list_id)
    },
    blockDelete(id){
      this.select_block_data.splice(id, 1);
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
            if(Math.abs(Number(surplus)) <= count){
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
.road_design{
  position: relative;
  padding-top: 400px;
  .canvas-parent {
    position: absolute;
    display: block;
    margin: auto;
    top: 0;
    left: 0;
    right: 0;
    width: 1200px;
    height: 400px;
  }
  .block-lists {
    margin-top: 40px;
    .button {
      width: 230px;
      display: flex;
      flex-wrap: nowrap;
      align-items: center;
      justify-content: center;
      margin: 2px;
      padding: 2px;
      margin-left: 18px;
      background: #262626;
      border-radius: 20px;
      .icon {
        display: block;
        text-align: center;
        font-size: 14px;
        padding: 6px;
        cursor: pointer;
      }
      .text{
        width: 140px;
        padding-left: 10px;
      }
      &:hover{
        background: rgb(34, 34, 34);
      }
    }
  }
}
</style>
