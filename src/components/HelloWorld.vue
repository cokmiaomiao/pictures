
<template>
  <div class="content">
    <div v-for="(item,i) in picArr" :key="i">
      <div id="img_div" v-if="item.pic" :class="item.ifMoveable?'moveable':'unmoveable'" :style="{transform:'scale('+ item.pscale+') rotate('+item.rotate+'deg)',zIndex:item.index,left:item.transtionx+'px',top:item.transtiony+'px'}" @touchstart="touchstartMove($event,i)" @touchmove="touchmoveMove($event,i)">
        <img :src="item.src" alt="">
        <div v-if="item.ifMoveable" class="control">
          <div class="zoom" @touchstart="touchstartScale($event,i)" @touchmove="touchmoveScale($event,i)">缩放</div>
          <div class="rotate" @touchstart="touchstartRotate($event,i)" @touchmove="touchmoveRotate($event,i)">旋转</div>
          <div class="deletePic" @touchend="deletePic($event,i)">删除</div>
          <div class="pres" @touchend="pres($event,i)">保存</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
/* eslint-disable */
export default {
  name: 'HelloWorld',
  data () {
    return {
      zIndex:0,
      picArr:[
        { 
          src:'https://ss1.bdstatic.com/70cFuXSh_Q1YnxGkpoWK1HF6hhy/it/u=2246402375,50345832&fm=26&gp=0.jpg',
          ifMoveable:true,
          pic:true,
          pscale:1,
          x:0,
          y:0,
          transtionx:0,
          transtiony:0,
          firstPositionx:0,
          firstPositiony:0,
          firstScalex:0,
          firstScaley:0,
          rotate:0,
          index:1,
          positionx:0,
          positiony:0,

        },
        {
          src:'https://ss0.bdstatic.com/70cFvHSh_Q1YnxGkpoWK1HF6hhy/it/u=3958849898,868063064&fm=26&gp=0.jpg',
          ifMoveable:true,
          pic:true,
          pscale:1,
          x:0,
          y:0,
          transtionx:0,
          transtiony:0,
          firstPositionx:0,
          firstPositiony:0,
          firstScalex:0,
          firstScaley:0,
          rotate:0,
          index:0,
          positionx:0,
          positiony:0,
        },

      ],
      
    }
  },
  methods:{
    //移动
    touchstartMove(e,i){
      e.preventDefault();
      if(this.picArr[i].ifMoveable){
        //调整层级
        this.zIndex++;
        this.picArr[i].index=this.zIndex;
        //获取初始点击位置
        this.picArr[i].x=e.targetTouches[0].pageX;
        this.picArr[i].y = e.targetTouches[0].pageY;
        //保存初始点击位置
        this.picArr[i].firstPositionx=this.picArr[i].transtionx;
        this.picArr[i].firstPositiony=this.picArr[i].transtiony;
      }
    },
    touchmoveMove(e,i){
      e.preventDefault();
      if(this.picArr[i].ifMoveable){
        //获取滚动后的鼠标位置
        let x=e.targetTouches[0].pageX;
        let y=e.targetTouches[0].pageY;
        //计算相对位移
        this.picArr[i].transtionx=this.picArr[i].firstPositionx+(x-this.picArr[i].x);
        this.picArr[i].transtiony=this.picArr[i].firstPositiony+(y-this.picArr[i].y);
      }
    },
    //缩放
    touchstartScale(e,i){
      e.preventDefault();
      e.stopPropagation();
      this.picArr[i].x = e.targetTouches[0].pageX;
      this.picArr[i].y = e.targetTouches[0].pageY;
    },
    touchmoveScale(e,i){
      e.preventDefault();
      e.stopPropagation();
      if(e.targetTouches[0].pageX-this.picArr[i].x!=0){
        let x=e.targetTouches[0].pageX;
        if(x-this.picArr[i].x>0){
          if(this.picArr[i].pscale<0.2){
            this.picArr[i].pscale=0.2
          }
          this.picArr[i].pscale=this.picArr[i].pscale-0.01; 
        }else{
          if(this.picArr[i].pscale>2){
            this.picArr[i].pscale=2
          }
          this.picArr[i].pscale=this.picArr[i].pscale+0.01
        }  
      }else if(e.targetTouches[0].pageY-this.picArr[i].y!=0){
        let y=e.targetTouches[0].pageY;
      }
    },
    //旋转
    touchstartRotate(e,i){
      e.preventDefault();
      e.stopPropagation();
      this.picArr[i].rotatex = e.targetTouches[0].pageX;
      this.picArr[i].rotatey = e.targetTouches[0].pageY;
    },
    touchmoveRotate(e,i){
      e.preventDefault();
      e.stopPropagation();
      let x=e.targetTouches[0].pageX;
      let y=e.targetTouches[0].pageY;
      let num=(x-this.picArr[i].rotatex);
      let num2=(y-this.picArr[i].rotatey)
      this.picArr[i].rotate=360*Math.atan(num2/num)/(2*Math.PI)
    },
    //删除
    deletePic(e,i){
      e.stopPropagation();
      this.picArr[i].pic=false;
    },
    //保存
    pres(e,i){
      e.stopPropagation();
      this.picArr[i].ifMoveable=false;
    },
  },
  mounted(){
  }

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
img{
  width: 100%;
}
#img_div{
  position: relative;
}
.moveable{
  position: absolute;
  padding: 1.2rem;
  user-select:none;
  width: 15rem;
}
.unmoveable{
  position: absolute;
  width: 15rem;
  padding: 1.3rem;
}
/* 控制器 */
.zoom{
  position: absolute;
  top: 0;
  left: 0;
}
.rotate{
  position: absolute;
  bottom: 0;
  right: 0;
}
.deletePic{
  position: absolute;
  top: 0;
  right: 0;
}
.pres{
  position: absolute;
  bottom: 0;
  left: 0;
}
</style>
