<template>
  <div id="app">
    <h1>{{ msg }}</h1>
    <div id="wrap">
      <ul>
        <li v-for="data in copysrcs"><img :src="data.src"/></li>
      </ul>
    </div>
    <div class="btn">
      <span v-for="(btn,index) in btns" @click="change(index)" :key="index" onselectstart="return false">{{btn.data}}</span>
    </div>
  </div>
</template>

<script>
  import Vue from 'vue'
  Vue.prototype.autoSwitch = function(){
      this.start = true;
      const _this = this;
      _this.timer = setInterval(function(){
        _this.num++;
        if (_this.num == _this.srcs.length) {
            _this.num = 0;
        }
        _this.copysrcs[0].src = _this.srcs[_this.num].src;
      },1000)
  };
export default {
  name: 'app',
  data () {
      return {
          num:0,
          msg: '点击切换图片',
          srcs: [ {src:"../src/assets/1.jpg"}, {src:'../src/assets/2.jpg'},{src:'../src/assets/3.jpg'},{src:'../src/assets/4.jpg'}],
          copysrcs:[{src:"../src/assets/1.jpg"}],
          btns:[ {data:"上一张"},{data:"下一张"},{data:"循 环"},{data:"停 止"}]
      }
  },
  methods:{
     change:function(type){
        if(type==0){ //向上
          this.num--;
          if(this.num==-1){
            this.num=this.srcs.length-1;
          }
        };
       if(type==1) { //向下
         this.num++;
         if (this.num == this.srcs.length) {
           this.num = 0;
         }
       }
       clearInterval(this.timer);
       if(this.start && type!=3){ //阻止上一张下一张与轮播冲突
         const _this = this;
         setTimeout(function(){
           _this.autoSwitch();
         },300);
       }
       this.copysrcs[0].src = this.srcs[this.num].src;
       if(type==2){ //循环
         if(!this.start)this.autoSwitch();
       }
    }
  }
}
</script>

<style>
  ul,li,div,img,p,span{margin: 0;padding: 0;}
  #app{
    width: 400px;
    margin: 0 auto;
    text-align: center;
  }
  #wrap {
    width: 390px;
    height: 220px;
    border:1px solid #5890AD;
    overflow: hidden;
  }

  h1{
    font-weight: normal;
  }
  ul {
    list-style-type: none;
    padding: 0;
    width: 1000px;
    overflow: hidden;
  }
  ul li,ol li{
    float: left;
  }
  .btn span{
    display: inline-block;
    background: #0b97c4;
    color: #fff;
    margin: 10px;
    font-size:14px;
    padding: 6px 10px;
    cursor: pointer;
    -moz-user-select:none;
  }
</style>
<!--
    点击上一张下一张进行向上向下切换、
    点击循环进行循环播放、
    点击暂停停止轮播、
    轮播开始点击向上或者向下之后循环依旧会继续、
    onselectstart="return false" 阻止双击默认选中文字
-->
