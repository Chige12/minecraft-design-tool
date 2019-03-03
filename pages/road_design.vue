<template lang="pug">
.road_design
  .view_box
    .view_canvas
      Canvas(:blocks="select_block_data" ref="canvas" :road="road").canvas-parent
    .view_setting
      Setting(@settingUpdate="settingUpdate" :road="road")
  .block-lists
    .block-list(v-for="(sel_block,sel_block_id) in select_block_data" :key="`key-${sel_block_id}`")
      BlockSelect(@blockSelect="blockSelect" :block="sel_block")
      .percent
        v-slider(v-model="sel_block.per" :label="`${sel_block.per}%`" @change="blockSelect(sel_block)" thumb-label :disabled="disabled")
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
import Setting from '~/components/Setting.vue'
import BlockData from '~/assets/json/block_data.json'

export default {
  components: {
    BlockSelect,
    Canvas,
    Setting
  },
  mounted(){
  },
  data(){
    return{
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
      disabled: true,
      road:{
        wid: 3,
        leng: 24
      }
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
    },
    settingUpdate(property,data){
      this.$refs.canvas.settingUpdate(property,data);
    }
  }
}
</script>
<style lang="scss">
.road_design{
  position: relative;
  height: auto;
  .view_box {
    display: flex;
    flex-wrap: nowrap;
    .view_canvas {
      height: auto;
      flex-shrink: 0;
    }
    .view_setting {
      width: 100%;
      height: 100%;
    }
  }
  .block-lists {
    margin-top: 40px;
    .block-list {
      display: flex;
      flex-wrap: nowrap;
      padding: 0 18px;
      .percent {
        padding-left: 18px;
        padding-top: 8px;
        width: 100%;
        .v-input--slider{
          margin: 0;
          .v-input__slot{
            margin: 0;
            .v-label {
              width: 45px;
            }
          }
          .v-messages{
            display: none;
          }
        }
      }
    }
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
      cursor: pointer;
      .icon {
        display: block;
        text-align: center;
        font-size: 14px;
        padding: 6px;
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
