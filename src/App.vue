<template>
  <div id="app">
    <ul class="list">
      <li 
        class="item"
        :class=" index === targetIndex && {'item-after': after,'item-before': before}"
        v-for="(item,index) in list" :key="item.name"
        @dragend="ondragend(index,$event)"
        @dragenter="ondragenter(index,$event)"
        @dragover="ondragover(index,$event)"
        draggable="true"
      >
        <img
          draggable="false"
          class="image"
          :src="item.src">
        <p class="name">{{item.name}}</p>
      </li>
    </ul>
  </div>
</template>

<script>
import pic1 from '@/assets/1.jpg'
import pic2 from '@/assets/2.jpg'
import pic3 from '@/assets/3.jpg'
import pic4 from '@/assets/4.jpg'
import pic5 from '@/assets/5.jpg'
import pic6 from '@/assets/6.jpg'
import pic7 from '@/assets/7.jpg'
import pic8 from '@/assets/8.jpg'


export default {
  name: 'app',
  data(){
    return {
      list: [
        {
          name: '图片一', src: pic1
        },
        {
          name: '图片二', src: pic2
        },
        {
          name: '图片三', src: pic3
        },
        {
          name: '图片四', src: pic4
        },
        {
          name: '图片五', src: pic5
        },
        {
          name: '图片六', src: pic6
        },
        {
          name: '图片七', src: pic7
        },
        {
          name: '图片八', src: pic8
        },

      ],
      targetIndex:'',
      cut:'',
      offsetX:0,
      after:false,
      before:false
    }
  },
  mounted(){
  },
  methods:{
    // 本身就是依靠事件触发的
    ondragend(index){
      if (this.targetIndex === index) return
        //需要判断
        // 把拖动的这一项剪切出来
        this.cut = this.list.splice(index,1)
      if (this.targetIndex > index){
        this.offsetX>=200?this.targetIndex = this.targetIndex:this.targetIndex--
        this.list.splice(this.targetIndex,0,this.cut[0])
      } else {
        this.offsetX>=200?this.targetIndex = this.targetIndex:this.targetIndex--
        this.list.splice(this.targetIndex+1,0,this.cut[0])
      }
      this.before = false
      this.after = false
    },
    ondragenter(index){
      if (this.targetIndex === index) return
      this.targetIndex = index
    },
    ondragover(index){  
      // index 是当前的序号
      this.offsetX = event.offsetX
      // 放入一个特效，用伪类，
      if (this.offsetX < 200) {
        this.before = true
        this.after = false
      } else{
        this.after = true
        this.before = false
      }
    }
  }
}
</script>

<style>
body,html{
  margin:0;
  padding:0;
}
ul,li,p{
  margin:0;
  padding:0;
  list-style: none;
}
#app{
  width:100%;
}
#app .list{
  position:relative;
  display: flex;
  justify-content: flex-start;
  align-items: center;
  flex-wrap: wrap;
  width:100%;
}
#app .list .item{
  position: relative;
  width:400px;
  height:230px;
  margin:0 5px;
}
#app .list .item-before:before{
  position: absolute;
  top:50%;
  left:-3px;
  transform: translateY(-50%);
  display: block;
  content:'';
  width:3px;
  height:50%;
  background-color: orange;
}
#app .list .item-after:after{
  position: absolute;
  top:50%;
  right:-3px;
  transform: translateY(-50%);
  display: block;
  content:'';
  width:3px;
  height:50%;
  background-color: orange;
}
#app .list .item .image{
  display: block;
  width:100%;
  height:200px;
}
#app .list .item .name{
  width:100%;
  height:30px;
  text-align: center;
}
</style>
