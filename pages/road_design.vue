<template lang="pug">
  v-layout(fill-height class="pa-3 mb-2")
    .block-list
      BlockSelect(@blockSelect="blockSelect")

    Canvas(:blocks="select_block_data")
</template>
<script>
import BlockSelect from '~/components/BlockSelect.vue'
import Canvas from '~/components/Canvas.vue'

export default {
  components: {
    BlockSelect,
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
      select_block_data:[]
    }
  },
  methods:{
    blockSelect(block){
      
    }
  }
}
</script>
<style lang="scss">
.list-box {
  width: 200px;
  
}
</style>
