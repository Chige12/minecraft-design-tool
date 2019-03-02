<template lang="pug">
  .blockSelector
    .blockData-list
      .list-openButton(@click="blockSelectorOpen()")
        .block-image
          img.image(:src="`../block/${block.id}.png`")
        .block-text
          .jpName {{block.jp}}
          .enName {{block.name}}
    .selector-lists(v-if="selector_list_open")
      .selector-list(v-for="(list_block,list_block_id) in block_data" :key="`key-${list_block_id}`" :class="{'active':(block.id==list_block.id)}")
        .select_button(@click="blockSelect(list_block_id)")
          .list_block_image
            img.image(:src="`../block/${list_block.id}.png`")
          .list_block-text
            .name {{list_block.jp}}
</template>
<script>
import BlockData from '~/assets/json/block_data.json'

export default {
  props:["block"],
  data(){
    return{
      selector_list_open: false,
      block_data: BlockData,
      
    }
  },
  methods:{
    blockSelectorOpen(){
      if(this.selector_list_open===false){
        this.selector_list_open = true
      }else{
        this.selector_list_open = false
      }
    },
    blockSelect(id){
      this.block.id = this.block_data[id].id
      this.block.name = this.block_data[id].name
      this.block.jp = this.block_data[id].jp
      this.selector_list_open = false
      this.$emit('blockSelect',this.block);
    }
  }
}
</script>
<style lang="scss">
.blockSelector{
  position: relative;
  padding: 0 18px;
  .blockData-list{
    .list-openButton{
      display: flex;
      flex-wrap: nowrap;
      align-items: center;
      width: 230px;
      padding: 4px;
      height: auto;
      cursor: pointer;
      .block-image{
        flex-shrink: 0;
        height: 40px;
        img.image {
          display: inline-block;
          width: 40px;
          height: 40px;
          image-rendering:pixelated;
        }
      }
      .block-text{
        padding-left: 12px;
        width: auto;
      }
      &:hover{
        background: #222;
      }
    }
  }
  .selector-lists{
    position: absolute;
    top: auto;
    left: 18px;
    z-index: 1000;
    background: #222;
    margin: auto;
    width: 230px;
    padding: 8px 0;
    .selector-list {
      padding: 2px 16px;
      .select_button {
        display: flex;
        flex-wrap: nowrap;
        align-items: center;
        cursor: pointer;
        .list_block_image {
          img.image {
            display: block;
            width: 20px;
            height: 20px;
            image-rendering:pixelated;
          }
        }
        .list_block-text {
          padding-left: 12px;
        }
      }
      &:hover{
        background: #151515;
      }
    }
    .active{
      background: #121212;
    }
  }
}

</style>

