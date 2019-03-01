<template lang="pug">
  v-layout(fill-height class="pa-3 mb-2")
    .list-box
      .list(v-for="(block,block_id) in block_data" :key="`key-${block_id}`")
        .block_button(@click="")
          .block_image
            img.image(:src="`../block/${block.id}.png`")
          .block-data
            .name {{block.name}}
            .jp {{block.jp}}
    Canvas(:blocks="block_data")
</template>
<script>
import Canvas from '~/components/Canvas.vue'
import BlockData from '~/assets/json/block_data.json'

export default {
  components: {
    Canvas
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
      block_data:BlockData
    }
  }
}
</script>
<style lang="scss">
.list-box {
  width: 200px;
  .list {
    .block_button {
      display: flex;
      flex-wrap: nowrap;
      .block_image {
        img.image {
          display: inline-block;
          width: 40px;
          height: 40px;
          image-rendering:pixelated;
        }
      }
      .block-data {
        padding-left: 12px;
      }
    }
  }
}
</style>
