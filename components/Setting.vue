<template lang="pug">
  .setting
    BlockSelect(@blockSelect="blockSelect" :block="back_img_data")
    
</template>
<script>
import BlockSelect from '~/components/BlockSelect.vue'
export default {
  components:{
    BlockSelect
  },
  data(){
    return{
      back_img_data:{
        list_id: 0,
        id: "grass_plains",
        name: "Grass",
        jp: "草ブロック",
        per: 0
      },
      road:{
        wid: 3,
        leng: 24
      },
      biomes:["plains","forest","desert","swamp","tundra"],
    }
  },
  methods:{
    blockSelect(block){
      if(block.id=='grass_plains'){
        //biome change
      }
      this.$emit('settingUpdate','block',block);
    }
  }
}
</script>
