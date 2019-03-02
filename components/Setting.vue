<template lang="pug">
  .setting
    .setting-property.setting-backimg
      .setting-property-name 背景画像の変更
      BlockSelect(@blockSelect="blockSelect" :block="back_img_data")
    .setting-property.setting-biomes
      v-select(v-model="biome" :items="biomes" label="バイオーム" @change="biomeChange()")
    .setting-property.setting-road
      .road_wid
        v-text-field(v-model.number="road.wid" label="道幅" type="number" @change="roadWidChange()")
      .road_leng
        v-text-field(v-model.number="road.leng" label="道の長さ" type="number" @change="roadLengChange()")
    
</template>
<script>
import BlockSelect from '~/components/BlockSelect.vue'
export default {
  components:{
    BlockSelect
  },
  props:["road"],
  data(){
    return{
      back_img_data:{
        list_id: 0,
        id: "grass_plains",
        name: "Grass",
        jp: "草ブロック",
        per: 0
      },
      biomes:["plains","forest","desert","swamp","tundra"],
      biome:"plains"
    }
  },
  methods:{
    blockSelect(block){
      this.$emit('settingUpdate','block',block);
    },
    roadWidChange(){
      this.$emit('settingUpdate','road_wid',this.road.wid);
    },
    roadLengChange(){
      this.$emit('settingUpdate','road_leng',this.road.leng);
    },
    biomeChange(){
      this.$emit('settingUpdate','biome',this.biome);
    }
  }
}
</script>
<style lang="scss">
.setting{
  padding: 20px;
  .setting-property {
    padding: 12px 0;
  }
  .setting-road{
    display: flex;
    flex-wrap: nowrap;
    .road_leng{
      padding-left: 12px ;
    }
  }
      
}
</style>
