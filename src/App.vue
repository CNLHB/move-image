<template>
  <div id="app">
    <!-- <div class="bg_img"></div> -->
    <div class="img_box" >
      <MoveImg ref="moveImgRef"  :imgs="imgs" :options="options"/>
    </div>
     <div class="box">
      <div class="box_btnBox">
        <button class="leftBtn" @click="toLeft">向左</button>
         <button class="addSpeedBtn" @click="addSpeed">加速</button>
        <button class="stopBtn" @click="stop">暂停</button>
      </div>
      <div class="box_btnBox">
        <button class="rightBtn" @click="toRight">向右</button>
        <button class="subSpeedBtn" @click="subSpeed">减速</button>
        <button class="restoreBtn" @click="reset">还原</button>
      </div>
    </div>
  </div>
</template>

<script>
import MoveImg from './components/MoveImage.vue'

export default {
  name: 'App',
  components: {
    MoveImg
  },
  data() {
    return {
      moveImgRef: null,
      imgs: [require('./assets/方案一/背景动.png')],
      options: {
        max: 100,
        stage: 1,
        speed: 5,
        autoplay: true,
        width: '560px',
        height: '484px'
      }
    }
  },
  mounted(){
      this.moveImgRef = this.$refs.moveImgRef

  },
  methods: {
    start() {
      console.log(this.moveImgRef);
      this.moveImgRef.start()
    },
    stop() {
      this.moveImgRef.stop()
    },
    addSpeed() {
      this.moveImgRef.addSpeed()
    },
    subSpeed() {
      this.moveImgRef.subSpeed()
    },
    toLeft() {
      this.moveImgRef.toLeft()
    },
    toRight() {
      this.moveImgRef.toRight()
    },
    reset() {
      this.moveImgRef.reset()
    },
  }
  
}
</script>

<style lang="scss">
html,body {
  margin: 0;
  padding: 0;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  position: relative;
}
.bg_img{
  position: absolute;
  top: 0;
  // background: url('./assets/方案一/背景不动.png') no-repeat;
  // background-size: 100% 100%;
  // width: 100%;
  // height: 477px;
}

.img_box{
  width: 100%;
  height: 500px;
  top: 100px;
  position:relative;
}
.box{
  position: fixed;
  bottom: 10px;
  width: 100%;
  overflow: hidden;
  z-index: 99;
}
      .box_btnBox {
        width: 100%;
        display: flex;
        align-items: center;
        justify-content: space-between;
        flex-wrap: wrap;
        margin-top: 5px;
      }
      .box_btnBox button {
        padding: 5px 20px;
      }
</style>
