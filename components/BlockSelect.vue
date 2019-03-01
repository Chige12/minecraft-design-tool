<template lang="pug">
  .blockSelector
    .selector
      .list-openButton(@click="blockSelectorOpen()")
        .block-image
          img.image(:src="`../block/${block.id}.png`")
        .block-text
          .jpName {{block.jp}}
          .enName {{block.name}}
      .selector-lists(v-if="selector_list_open")
        .selector-list(v-for="(list_block,list_block_id) in block_data" :key="`key-${list_block_id}`")
          .select_button(@click="blockSelect(list_block_id)")
            .list_block_image
              img.image(:src="`../block/${list_block.id}.png`")
            .list_block-text
              .name {{list_block.jp}}
      .percent
        v-slider(v-model="block.per" :label="`${block.per}%`" inverse-label)

</template>
<script>
import BlockData from '~/assets/json/block_data.json'

export default {
  data(){
    return{
      block: {
        id: "grass_path_top",
        name: "Grass Path",
        jp: "小道",
        per: "80"
      },
      selector_list_open: false,
      block_data: BlockData
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
      this.block.per = this.block_data[id].per
      this.selector_list_open = false
      this.$emit('blockSelect',this.block);
    }
  }
}
</script>
<style lang="scss">
.blockSelector{
  .selector{
    .list-openButton{
        display: flex;
        flex-wrap: nowrap;
        cursor: pointer;
      .block-image{
        img.image {
          display: inline-block;
          width: 40px;
          height: 40px;
          image-rendering:pixelated;
        }
      }
      .block-text{
        padding-left: 12px;
      }
    }
    .selector-lists{
      position: relative;
      .selector-list {
        .select_button {
          display: flex;
          flex-wrap: nowrap;
          cursor: pointer;
          .list_block_image {
            img.image {
              display: inline-block;
              width: 20px;
              height: 20px;
              image-rendering:pixelated;
            }
          }
          .list_block-text {
            padding-left: 12px;
          }
        }
      }
    }
    .percent {
      .v-input--slider{
        margin: 0;
        .v-input__slot{
          margin: 0;
          .v-label {
            width: 45px;
          }
        }
      }
    }
  }
}

</style>

