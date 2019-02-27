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
      block_data:[
        {
          id:"grass_path_top",
          name:"Grass Path",
          jp:"小道",
          per:"80"
        },
        {
          id:"grass_plains",
          name:"Grass",
          jp:"草",
          per:"10",
        },
        {
          id:"coarse_dirt",
          name:"Coarse Dirt",
          jp:"荒土",
          per:"10"
        },
        {
          id:"cobblestone",
          name:"Cobble Stone",
          jp:"丸石",
          per:"0"
        },
        {
          id:"gravel",
          name:"Gravel",
          jp:"砂利",
          per:"0"
        },
        {
          id:"stone_bricks",
          name:"Stone Bricks",
          jp:"石レンガ",
          per:"0"
        },
        {
          id:"cracked_stone_bricks",
          name:"Cracked Stone Bricks",
          jp:"ひび割れた石レンガ",
          per:"0"
        },
        {
          id:"granite",
          name:"Granite",
          jp:"花崗岩",
          per:"0"
        },
        {
          id:"polished_granite",
          name:"Polished Granite",
          jp:"磨かれた花崗岩",
          per:"0"
        },
        {
          id:"andesite",
          name:"Andesite",
          jp:"安山岩",
          per:"0"
        },
        {
          id:"polished_andesite",
          name:"Polished Andesite",
          jp:"磨かれた安山岩",
          per:"0"
        },
        {
          id:"diorite",
          name:"Diorite",
          jp:"閃緑岩",
          per:"0"
        },
        {
          id:"polished_diorite",
          name:"Polished Diorite",
          jp:"磨かれた閃緑岩",
          per:"0"
        },
        {
          id:"mossy_cobblestone",
          name:"Mossy Cobble Stone",
          jp:"苔生した丸石",
          per:"0"
        },
        {
          id:"mossy_stone_bricks",
          name:"Mossy Stone Bricks",
          jp:"苔生した石レンガ",
          per:"0"
        }
      ]
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
