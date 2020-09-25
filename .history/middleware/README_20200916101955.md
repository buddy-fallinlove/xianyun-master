<template>
  <div>
    
    <a-carousel autoplay>
      <div v-for="(item,index) in res" :key="index">
        <img :src="`http://157.122.54.189:9095${item.url}`" alt />
        
      </div>
    </a-carousel>
    <div class="dd">
        <div class="yy">
           <div
          v-for="(item1,index1) in arr"
          :key="index1"
          @click="font(index1)"
          :class="(active==index1)?'actives':''"
        >
        <div class="active">{{item1}}</div>
      </div>
        </div>
        
      
      <div class="ii">
        <a-input-search
      v-model:value="value"
      :placeholder="placeholder"
      style="width: 550px height: 50px"
      @search="onSearch"
    />
      </div>
      
    </div>
    
  </div>
</template>

<script lang='ts'>
import {
  defineComponent,
  reactive,
  toRefs,
  SetupContext,
  onMounted,
} from "vue";
import api from "@tp/api";
import { listItem, Result } from "@pespe";

interface Data {
  res: listItem[];
  value: string;
   arr: string[];
  active: number;
  placeholder: string;
}
export default defineComponent({
  name: "",
  props: {},
  components: {},
  setup(props, ctx: SetupContext) {
    const data: Data = reactive<Data>({
      res: [],
      value:'',
      arr: ["攻略", "酒店", "机票"],
      active: 0,
      placeholder: "搜索城市",
    });
    onMounted(() => {
      api
        .getIndex()
        .then((res: Result) => {
          data.res = res.data;
          console.log(res);
        })
        .catch((err) => {
          console.log(err);
        });
    });
    const  onSearch = (value: string): void=>{
      console.log('use value', value)
    }
    const font = (index1: number): void=>{
      data.active = index1;
      if(index1==0){
          data.placeholder="搜索城市"
      }
      if(index1==1){
          data.placeholder="请输入城市搜索酒店"
      }
      if(index1==2){
          data.placeholder="请输入出发地"
      }
    }

    return {
      ...toRefs(data),
      onSearch,
      font
    };
  },
});
</script>

<style scoped lang='scss'>
.yy{
  display: flex;
}
.active {
  font-size: 16px;
  display: block;
  color: #fff;
  height: 0;
  width: 90px;
  line-height: 42px;
  border-width: 0px 10px 42px 0px;
  border-style: none solid solid;
  border-color: transparent transparent rgba(0, 0, 0, 0.3);
  background: transparent;
  text-align: center;
}
.actives {
  font-size: 16px;
  display: block;
  color: #000;
  height: 0;
  width: 90px;
  line-height: 42px;
  border-width: 0px 10px 42px 0px;
  border-style: none solid solid;
  border-color: transparent transparent rgb(238, 238, 238);
  background: transparent;
  text-align: center;
}
.ii{
  height: 500px;
  width: 500px;
  position: absolute;
  top: 50px;

}
.ant-carousel ::v-deep(.slick-slide) {
  text-align: center;
  height: 600px;
  line-height: 600px;
  background: #364d79;
  overflow: hidden;
}
.ant-carousel ::v-deep(.slick-slide h3) {
  color: #fff;
}
.dd {
     position: absolute;
     top: 250px;
    
     left: 400px;
     
     
}
.ss {
  display: flex;
}
// .price {
//   font-size: 15px;
//   color: #fff;
//   height: 0;
//   width: 109px;
//   line-height: 52px;
//   padding-left: 5px;
//   border-width: 0px 12px 52px 0px;
//   border-style: none solid solid;
//   border-color: transparent transparent #333333;
//   opacity: 0.5;
// }
</style>
