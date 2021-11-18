<template>
  <!-- 抽奖的游戏机台 -->
<div class="wheel-framework-wrap">
  <div class="framework-wrap">
    <img src="../src/assets/framework.png" alt="">
  </div>
  
  <div class="wheel-wrap-wrap">
  <!-- 抽奖转盘 -->
  <div class="wheel-wrap" :style="`transform: rotate(${wheelDeg}deg)`">
    <!-- v-for循环根据奖品信息生成对应角度的扇形并放置红包黄包 -->
    <!-- 扇形生成方法：
    过半圆圆心的矩形与该半圆相交形成。
    半圆为同样的矩形设置border-radius生成。
                                                                       .*=.                   
                                                                  =@@@@#-                 
                                                                .#@@@@@@@@*:              
 %@@@@@@@@@@@@@@*   .:=*%@@@@@@@-                              -@@@@@@@@----:.            
 @@@@@@@@@@@@@@@*        :*@@@@@-                             *@@@@@@@@@         .        
 @@@@@@@@@@@@@@@*           +@@@-                           :%@@@@@@@@@@       .#@#:      
 @@@@@@@@@@@@@@@*            .%@-                          +@@@@@@@@@@@@      -@@@@@#.    
 @@@@@@@@@@@@@@@*              %-                        :%@@@@@@@@@@@@@     *@@@@@@@%.   
 @@@@@@@@@@@@@@@*              .-                       +@@@@@@@@@@@@@@@   -@@@@@@@@@@%   
 @@@@@@@@@@@@@@@*               .                     :%@@@@@@@@@@@@@@@@  *@@@@@@@@@@@@+  
 @@@@@@@@@@@@@@@*               .                    =@@@@@@@@@@@@@@@@@%:%@@@@@@@@@@@@@%  
 @@@@@@@@@@@@@@@*               .                  .#@@@@@@@@@@@@@@@@@@=*@@@@@@@@@@@@@@@  
 @@@@@@@@@@@@@@@*               .                 -@@@@@@@@@@@@@@@@@@%. @@@@@@@@@@@@@@@#  
 @@@@@@@@@@@@@@@*              --                 .=%@@@@@@@@@@@@@@@=   @@@@@@@@@@@@@@@:  
 @@@@@@@@@@@@@@@*             -@-                    .+@@@@@@@@@@@%.    @@@@@@@@@@@@@@=   
 @@@@@@@@@@@@@@@*            +@@-                       :*@@@@@@@+      @@@@@@@@@@@@@-    
 @@@@@@@@@@@@@@@*          =%@@@-                          -#@@%:       @@@@@@@@@@@*.     
 @@@@@@@@@@@@@@@*       -*@@@@@@-                            .-         @@@@@@@@%=.       
 *##############+ ..:=+#########:                                       @@@%#+-.          
                                                                                           -->

  
    <div v-for="(item, index) in prizeInfo" :key="index">
      <!-- 生成半圆的矩形，即上图右边的矩形 -->
      <div class="sector" :style=this.initializeRotateDegree(index)>
        <!-- 截取半圆的矩形，即上图左边的矩形 -->
        <!-- 绑定style：根据奖品数量（包括谢谢参与）设置扇形角度（改变矩形旋转角度） -->
        <!-- v-if判断高亮序号：转盘停止旋转后会高亮指针指向的扇形 -->
        <div class="sector-inner" v-if="this.prizeHighlight===index" 
        :style="`
        transform: translateX(-100px) rotate(${360/(prizeInfo.length+1)}deg);
        background:linear-gradient(90deg, rgba(255,220,160,1) 0%, rgba(255,153,115,1) 100%);
        `">
        <!-- 扇形上部的文字 -->
          <span class="prizeText"
          :style="`
          transform: 
          translateX(${10*(prizeInfo.length+1)-80}px) 
          translateY(${-10*(prizeInfo.length+1)+80}px) 
          rotate(${-180/(prizeInfo.length+1)}deg)
          `">现金</span>
          <!-- 扇形里的红包图片 -->
          <!-- 同样由奖品数量更改位置 -->
          <!-- 位置信息为手动测量出的坐标随数量改变的函数表达式（需优化） -->
          <div class="img-group"
          :style="`
          transform: 
          translateX(${10*(prizeInfo.length+1)-65}px) 
          translateY(${-5*(prizeInfo.length+1)+50}px) 
          rotate(${-180/(prizeInfo.length+1)}deg)
          `">
          <!-- 成品为红色黄色红包相间，故判断奇偶 -->
            <img class="envelope" v-if="index%2==0" src="../src/assets/redEnvelope.png">
            <img class="envelope" v-else src="../src/assets/yellowEnvelope.png">
            <div class="img-tip">￥{{item.prize_name}}</div>
          </div>
        </div>
        <!-- 未高亮扇形的状态 -->
        <div class="sector-inner" v-else
        :style="`
        transform: translateX(-100px) rotate(${360/(prizeInfo.length+1)}deg);`">
          <span class="prizeText"
          :style="`
          transform: 
          translateX(${10*(prizeInfo.length+1)-80}px) 
          translateY(${-10*(prizeInfo.length+1)+80}px) 
          rotate(${-180/(prizeInfo.length+1)}deg)
          `">现金</span>
          <div class="img-group"
          :style="`
          transform: 
          translateX(${10*(prizeInfo.length+1)-65}px) 
          translateY(${-5*(prizeInfo.length+1)+50}px) 
          rotate(${-180/(prizeInfo.length+1)}deg)
          `">
            <img class="envelope" v-if="index%2==0" src="../src/assets/redEnvelope.png">
            <img class="envelope" v-else src="../src/assets/yellowEnvelope.png">
            <div class="img-tip">￥{{item.prize_name}}</div>
          </div>
        </div>
      </div>
    </div>
    <!-- 循环结束，最后生成谢谢参与的扇形 -->
    <!-- 旋转角度与高亮扇形同理 -->
     <div class="sector" :style=this.initializeThanks()>
        <div class="sector-inner" v-if="this.prizeHighlight===-1" 
        :style="`
        transform: translateX(-100px) rotate(${360/(prizeInfo.length+1)}deg);
        background:linear-gradient(90deg, rgba(255,220,160,1) 0%, rgba(255,153,115,1) 100%);
        `">
          <span class="thanks"
          :style="`
          transform: 
          translateX(${10*(prizeInfo.length+1)-80}px) 
          translateY(${-10*(prizeInfo.length+1)+80}px) 
          rotate(${-180/(prizeInfo.length+1)}deg)
          `">谢谢参与</span>
          <div class="img-group"
          :style="`
          transform: 
          translateX(${10*(prizeInfo.length+1)-60}px) 
          translateY(${-5*(prizeInfo.length+1)+55}px) 
          rotate(${-180/(prizeInfo.length+1)}deg)
          `">
            <img class="envelope" src="../src/assets/thank.png">
          </div>
        </div>
        <div class="sector-inner" v-else
        :style="`
        transform: translateX(-100px) rotate(${360/(prizeInfo.length+1)}deg);`">
          <span class="thanks"
          :style="`
          transform: 
          translateX(${10*(prizeInfo.length+1)-80}px) 
          translateY(${-10*(prizeInfo.length+1)+80}px) 
          rotate(${-180/(prizeInfo.length+1)}deg)
          `">谢谢参与</span>
          <div class="img-group"
          :style="`
          transform: 
          translateX(${10*(prizeInfo.length+1)-60}px) 
          translateY(${-5*(prizeInfo.length+1)+55}px) 
          rotate(${-180/(prizeInfo.length+1)}deg)
          `">
            <img class="envelope" src="../src/assets/thank.png">
          </div>
        </div>
      </div>
  </div>
  </div>
</div>
<div class="light-wrap">
  <!-- 转盘周围无限闪烁的装饰灯 -->
  <!-- 装饰灯颜色白红相间，故判断奇偶 -->
  <!-- lightSum设置灯数量，默认为20，如果需要根据奖品数量更改可添加methods -->
    <img id="light" src="../src/assets/light-background.png">
</div>

  <!-- 可点击的抽奖指针 -->
  <div @click="onClickRotate" class="pointer-wrap">
      <img src="../src/assets/pointer.png" alt="">
  </div>
  <!-- 跳转服务端按钮 -->
  <a href="https://qc4dgz.web.cloudendpoint.cn/">
    <button class="button" >
    服务端
    </button>
  </a>

  <!-- 中奖信息弹出的窗口，包括出现于消失动画 -->
  <!-- fade transition为遮罩层（即背景变暗） -->
  <!-- 使用数据showModal判断是否弹出中奖页面 -->
 <transition name="fade" appear>
  <div class="modal-overlay" v-if="showModal" @click="showModal = false"></div>
 </transition>
 <div class="modal-wrap">
  <div v-if="showModal">
    <!-- 观察给定demo，发现中奖页面主元素包括红包和红包中的纸，
    以及辅元素背景光晕、铜钱、纸币。
    并且每个元素均有动画，其中
    主元素动画：
      从页面 中上部出现 由小变大 移动至页面中部，并有纸的弹出红包动画以及红包的放大缩小（弹跳感）
    辅元素动画：
      铜钱、纸币：淡入 向外移动
      光晕：淡入淡出 从中心放大直至消失
    vue transition无法满足复杂动画，因此使用css keyframe动画 -->
    <img id="lottery-window" class="lottery-window-back" src="../src/assets/back.png">
    <div id="lottery-paper" class="lottery-window-paper" v-if="this.lotteryIndex!=-1">
      <img  src="../src/assets/paper.png">
      <div class="lotteryText"><span class="lotteryPrize">{{this.queryLotteryPrizeName}}</span>元现金</div>
    </div>
    <div id="lottery-paper" class="lottery-window-paper" v-else>
      <img  src="../src/assets/paper-thank.png">
      <div class="lotteryText">谢谢参与</div>
    </div>
    <img id="lottery-window" class="lottery-window-front" src="../src/assets/front.png">
  </div>
  <button id="close-button" class="close" v-if="showModal" @click="showModal = false">
    <img src="../src/assets/close.png">
  </button>
  <div v-if="showModal">
    <img id="left-icon" class="left-icon" src="../src/assets/left-icon.png">
    <img id="right-icon" class="right-icon" src="../src/assets/right-icon.png">
    <img id="glow-circle" class="glow-circle" src="../src/assets/glow-circle.png">
  </div>
</div>
</template>

<script>

export default {
  name: 'App',
  components: {
  },
  data: function(){
    return{
      prizeInfo: [],
      queryLottery: {},
      queryLotteryPrizeMSG: "",
      queryLotteryPrizeName:"",
      wheelDeg: 0,
      lotteryIndex: -1,
      unitDegree: 0,
      lightSum: 20,
      rolling: false,
      showModal: false,
      prizeHighlight: -2,
    }
  },
  methods:{
    // 获得奖品信息并保存在prizeInfo中（保存接口返回的response数组）
    // 改进：可以使用axios（在服务端中使用）
    async getPrizeInfo(){
      console.log("getPrizeInfo");
      // const prizeInfoOrigin = await fetch('https://qcaaph.fn.thelarkcloud.com/getPrizeInfo');
      const prizeInfoOrigin = await fetch('https://qciwmk.fn.thelarkcloud.com/getPrizeInfo');      
      const prizeInfoJSON = await prizeInfoOrigin.json();
      this.prizeInfo = prizeInfoJSON.response;
      console.log(this.prizeInfo);
    },
    // 获得抽奖信息并保存在queryLottery中（保存接口返回的完整对象，json格式）
    async getQueryLottery(){
      console.log("getQueryLottery");
      // const queryLotteryOrigin = await fetch('https://qcaaph.fn.thelarkcloud.com/queryLottery');
      const queryLotteryOrigin = await fetch('https://qciwmk.fn.thelarkcloud.com/queryLottery');
      const queryLotteryJSON = await queryLotteryOrigin.json();
      this.queryLottery = queryLotteryJSON;
      this.queryLotteryPrizeMSG = queryLotteryJSON.msg;
      if(typeof(this.queryLottery.response.prize.prize_name)==='undefined')
      {
        this.queryLotteryPrizeName = "";
      }
      else{
        this.queryLotteryPrizeName = this.queryLottery.response.prize.prize_name;
      }
      // console.log("queryLotteryInget:"+this.queryLottery);
      // console.log("queryLotteryResponseInget:"+this.queryLottery[1]);
      // console.log(typeof(this.queryLottery.response.prize.prize_name)==='undefined');
      // console.log("queryLotteryResponsePrizeNameInget:"+this.queryLotteryPrizeName);
      // console.log("queryLotterymsgInget:"+this.queryLotteryPrizeMSG);
    },
    // 初始化第i个扇形的旋转角度（根据奖品数量（包含谢谢参与））
    initializeRotateDegree(i){
      var style="transform: rotate(";
      var degree = i*(360/(this.prizeInfo.length+1));
      style = style+degree+"deg)";
      // console.log(style);
      return style;
    },
    // 初始化谢谢参与的扇形旋转角
    initializeThanks(){
      console.log("initializeThanks");
      var style="transform: rotate(";
      var degree = (360/(this.prizeInfo.length+1));
      style = style+"-"+degree+"deg)";
      // console.log(style);
      return style;
    },
    // 初始化第i个装饰灯的旋转角度（由lightSum确定）
    initializeLightDegree(i){
      console.log("initializeLightDegree");
      var style="transform: rotate(";
      var degree = i*(360/this.lightSum);
      style = style + degree +"deg)";
      return style;
    },
    // 点击抽奖指针的事件
    onClickRotate() {
      // 转盘是否在旋转，如果在旋转则关中断（不允许再次点击）
    if(this.rolling==false){
      this.rolling = true;
      // 初始化中奖的扇形的序号，-1为谢谢参与，其他中奖的扇形为lotteryIndex，<=-2则为无效果
      this.prizeHighlight = -2;
      // 首先获取中奖信息
      this.getQueryLottery().then(()=>{
        // console.log("queryLotteryInclick:" + JSON.stringify(this.queryLottery));
        // console.log("queryLotteryPrizeNameInRotate:" + this.queryLotteryPrizeName);
        // console.log("PrizeInfoInRotate:" + JSON.stringify(this.prizeInfo));
        // console.log("compareToUndefined:" + (this.queryLotteryPrizeName===this.prizeInfo[0].prize_name));
        // console.log("PrizeInfoInRotate:" + this.prizeInfo[0].prize_name);
        // 如果中奖信息为空，即谢谢参与
        if(this.queryLotteryPrizeName==="")
        {
          this.lotteryIndex = -1;
        }
        // 否则遍历奖品信息比对中奖信息找到对应的扇形序号
        else{
          for(let i = 0; i < this.prizeInfo.length;i++)
          {
            if(this.queryLotteryPrizeName===this.prizeInfo[i].prize_name)
            {
              this.lotteryIndex = i;
              break;
            }
            }
        }
        // 单位角度为每一个扇形的弧度，由360度/奖品数得到
        var unitDegree = (360/(this.prizeInfo.length+1));
        console.log("lotteryIndexInClick:" + this.lotteryIndex);
        console.log("unitDefree:" + unitDegree);
        console.log("beforeWheelDeg:"+(360-unitDegree*this.lotteryIndex-unitDegree/2));
        // 转盘旋转的角度，并且要最终停留在中奖的扇形上。
        const { wheelDeg } = this;
        this.wheelDeg = 
          // 上次转盘旋转的角度
          wheelDeg -
          // 对上次旋转角度求模，重新开始计算新旋转角
          wheelDeg % 360 +
          // 旋转6圈
          6 * 360 +
          // 最终停留角度为对应序号（在转盘中为逆序，故需减去）乘以单位扇形角，并加上半个单位角（使指针停留在扇形中间）
          // (360 - 360 / (this.prizeInfo.length+1) * this.lotteryIndex);
          360-unitDegree*this.lotteryIndex-unitDegree/2;
        console.log("wheelDeg:" + this.wheelDeg);
        setTimeout(() => {
          // 旋转状态开中断
          this.rolling = false;
          // 高亮中奖的扇形
          this.prizeHighlight = this.lotteryIndex;
          setTimeout(() => {
            this.prizeHighlight = -2;
            setTimeout(() => {
              // 弹出中奖信息窗口
              this.showModal = true;
            }, 200);
            
          }, 800);
          
          // 持续时间5.5s
        }, 5500);
      }
      )
      }
    },
  },

  created: function(){
    this.getPrizeInfo();
    this.unitDegree = (360/(this.prizeInfo.length+1));
    // console.log("createdUnitDegree:" + this.unitDegree);
    // this.getQueryLottery();
  },
  beforeCreated(){
    let count = 0;
    let imgs = [
      require('../src/assets/paper.png'),
      require('../src/assets/paper-thank.png')
    ];
    for (let img of imgs){
      let image = new Image();
      image.onload = () =>{
        count = count+1;
      };
      image.src = img;
    }
  }
}
</script>

<style>
/* 样式按照template顺序整理 */
#app {
  /* position: absolute; */
  /* font-family: Avenir, Helvetica, Arial, sans-serif; */
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
body{
  background:linear-gradient(to right, #ff9966, #ff5e62); /* W3C, IE 10+/ Edge, Firefox 16+, Chrome 26+, Opera 12+, Safari 7+ */;
}


.wheel-framework-wrap{
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%,-50%);
}


.framework-wrap{
  position: absolute; left: 50%; top: 50%;
  transform: translate(-50%, -50%) scale(3);
  z-index: 10;
}

.wheel-wrap-wrap{
  position: relative;
  left: 50%;
  top: -300px;
  transform: translate(-50%,-50%) scale(3)
}
.wheel-wrap{
  transform-origin: 0px 100px;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  transition: transform 5s ease-in-out;
}
.sector {
  position: absolute;
  width: 100px;
  height: 200px;
  border-radius: 0px 100px 100px 0;
  overflow: hidden;
  transform-origin: left center;
  /* transform: rotate(0deg); */
        }
.sector-inner {
  text-align: center;
  display: block;
  width: 40px;
  padding: 5px 3px 0 57px;
  height: 195px;
  /* background: #ffeab1; */
  /* transform: translateX(-100px) rotate(60deg); */
  transform-origin: right center;
  border-radius: 100px 0 0 100px;
  background: linear-gradient(90deg, rgba(255,244,225,1) 0%, rgba(254,227,190,1) 100%);
}
.prizeText{
  display: block;
  transform-origin:left center;
  color: #e27160;
  font-size: x-small;
}
.thanks{
  white-space: nowrap;
  display: block;
  transform-origin:left center;
  color: #e27160;
  font-size: x-small;
}
.img-group {
  position: relative;
  display: inline-block;
  transform-origin:left center;
}
.img-tip {
  font-size: xx-small;
  color: #ffeab1;
  position: absolute;
  bottom: 4px;
  left: 6px;
}
.envelope{
  width: 70%;
}

.light-wrap{
  /* color: black; */
  z-index: 30;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%,-50%) scale(3);
}
#light{
    animation: light-switch 1000ms;
    animation-iteration-count: infinite; 
}
@keyframes light-switch {
0% {background: url(../src/assets/light.png);}
45% {background:url(../src/assets/light.png);}
50% {background:url(../src/assets/light2.png);}
100% {background:url(../src/assets/light2.png);}
}

.pointer-wrap{
  overflow: hidden;
  /* transform-origin: left center; */
  position: absolute; left: 50%; top: 50%;
  transform: translate(-50%, -50%) scale(3);
  z-index:30;
}

.button {
 position: relative;
 appearance: none;
 outline: none;
 border: none;
 background: none;
 cursor: pointer;
 /* display: inline-flex; */
 display: inline-block;
 padding: 50px 75px;
 background-image: linear-gradient(to right, #CC2E5D, #FF5858);
 border-radius: 25px;
 
 color: #FFF;
 font-size: 50px;
 font-weight: 700;
 
 box-shadow: 3px 3px rgba(0, 0, 0, 0.4);
 transition: 0.4s ease-out;
 z-index: 79;
}
.close{
  position: absolute; left: 50%; top: 45.1%;
  transform: translateX(-50%) translateY(10vh);
  appearance: none;
  outline: none;
  border: none;
  background: none;
  cursor: pointer;
  opacity: 0.8;
  transition: 0.4s ease-out;
  z-index: 95;
}
#close-button{
  animation: close-button-enter 300ms;
  z-index: 95;
}
@keyframes close-button-enter {
  0% {transform: translate(-50%, 5vh);}
  100% {transform: translate(-50%, 10vh);}
}


.modal-overlay {
 position: absolute;
 top: 0;
 left: 0;
 right: 0;
 bottom: 0;
 z-index: 80;
 background-color: rgba(0, 0, 0, 0.6);

}
.fade-enter-active
/* .fade-leave-active { */
{
 transition: opacity 1s;
}
.fade-enter-from,
.fade-leave-to {
 opacity: 0;
}

.lottery-window{
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%,-50%);
}
.lottery-window-back{
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%,-50%);
}
.lottery-window-paper{
  z-index: 90;
  position: absolute;
  left: 50%;
  top: 49%;
  transform: translate(-50%,-50%);
}
.lottery-window-front{
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%,-50%);
}
#lottery-window{
  z-index: 90;
  animation: lottery-window-enter 500ms ;
}
@keyframes lottery-window-enter {
  0% {transform: translate(-50%, -80%) scale(0.2);}
  /* 60% {transform: translate(-50%, -40%);} */
  60% {transform: translate(-50%, -50%) scale(1.05);}
  100% {transform: translate(-50%, -50%) scale(1);}
}
#lottery-paper{
  animation: lottery-paper-enter 500ms ;
}
@keyframes lottery-paper-enter {
  0% {transform: translate(-50%, -80%) scale(0.2);}
  30% {transform: translate(-50%, -45%) scale(1) ;}
  60% {transform: translate(-50%, -55%) ;}
  100% {transform: translate(-50%, -50%) scale(1);}
}
.lotteryText{
  white-space: nowrap;
  z-index: 95;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%,-150%);
  font-size: 40px;
  color: #d84c22;
  display: inline-block;
  white-space: nowrap;
}
.lotteryPrize{
  font-size: 80px;
  color: #d84c22;
}

#left-icon{
  animation: left-icon-enter 500ms;
}
#right-icon{
  animation: right-icon-enter 600ms;
}
@keyframes left-icon-enter {
  0% {opacity: 0; transform: translate(-50%,-50%);}
  35% {opacity: 0; transform: translate(-50%,-50%);}
  100% {opacity: 1; transform: translate(-55%,-55%);}
}
@keyframes right-icon-enter {
  0% {opacity: 0; transform: translate(-50%,-50%);}
  40% {opacity: 0; transform: translate(-50%,-50%);}

  100% {opacity: 1; transform: translate(-40%,-55%);}
}
.left-icon{
 z-index: 90;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-55%,-55%);
}
.right-icon{
 z-index: 90;
  position: absolute;
  left: 49%;
  top: 50%;
  transform: translate(-40%,-55%);
}

.glow-circle{
  z-index: 85;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%,-50%) ;
  opacity: 0;
}
#glow-circle{
  animation: glow-circle-enter 600ms;
}
@keyframes glow-circle-enter {
  0% {opacity: 0; transform: translate(-50%,-50%) scale(0.5);}
  70% {opacity: 1; transform:translate(-50%,-50%) scale(1.8);}
  100% {opacity: 0; transform: translate(-50%,-50%) scale(1.9);}
}
.modal-wrap{
  z-index: 85;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%,-50%) scale(2);
}

</style>
