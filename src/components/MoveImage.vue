<template>
  <div class="container">
    <div class="box_img_box" :style="{ height: options.height }">
      <div class="imgs_box" ref="imgsBox" :style="{ height: options.height }">
        <img
          class="img_item"
          v-for="(item, index) in imgs"
          :key="index"
          :src="item"
          alt=""
          :style="{ width: options.width, height: options.height }"
        />
        <template v-if="imgs && imgs.length == 0">
          <div
            v-for="(item, index) in 10"
            :key="index"
            :class="'rect '"
            :style="{ background: randomColor() }"
          ></div>
        </template>
      </div>
    </div>
  </div>
</template>

<script>
let LEFT = "left";
let TRANSLATE = "translate";
class MoveImage {
  constructor(imgsBox, options = {}) {
    this.options = options
    // 获取图片容器
    this.imgsBox = imgsBox
    // 获取图片宽
    this.imgWidth =  this.imgsBox.clientWidth / this.imgsBox.children.length
    // 初始速度
    this.speed = options.speed || 5
    // 添加移动方向
    this.direction = options.direction || 'left'
    // 初始化加减步骤大小
    this.stage = options.stage || 1
    // 初始化最大速度
    this.max = options.max || 50
    // 添加定时器
    this.timer = null
    this.mode = options.mode || TRANSLATE
    this.left = 0
    // 初始化无缝滚动
    this.init()
  }
  addImg() {
    // 克隆第一张和最后一张图片
    const firstImg = this.imgsBox.children[0].cloneNode(true)
    const lastImg =
      this.imgsBox.children[this.imgsBox.children.length - 1].cloneNode(true)
    // 将图片插入图片容器最前面和最后面
    this.imgsBox.insertBefore(lastImg, this.imgsBox.children[0])
    this.imgsBox.appendChild(firstImg)
  }
  init() {
    // 增加图片
    this.addImg()
    // 矫正图片位置
    this.translate(-this.imgWidth)
  }
  start() {
    if (this.timer) {
      return
    }
    this.animation()

  }
  animation(){
    if(window.requestAnimationFrame){
      requestAnimationFrame(this.update.bind(this))
      this.timer = String(Math.random())
    }else{
        this.timer = setInterval(() => {
        this.update()
      }, 20)
    }

  }
  update(){
      if(this.timer){
        this.animation()
      }
          //  获取当前left
      let l = this.mode === LEFT ? this.imgsBox.offsetLeft : this.left
      if (this.direction === 'right') l += this.speed
      else l -= this.speed
      // 判断是否到第一张或最后一张

      if (l <= -(this.imgsBox.children.length - 1) * this.imgWidth) {
        const difference = l + ((this.imgsBox.children.length - 1) * this.imgWidth)
        this.translate(-this.imgWidth + difference)
        return
      } else {
        this.translate(l)
      }
      if (l >= 0) {
        const left = -(this.imgsBox.children.length - 2) * this.imgWidth
        this.translate(left)
        return
      } else {
        this.translate(l)
      }
  }
  translate(left) {
    if(this.mode === LEFT) {
      this.imgsBox.style.left = left + 'px'
    }else{
      this.imgsBox.style.transform = `translate3d(${left}px,0,0)`
    }
    this.left = left
  }
  stop() {
    this.clear()
  }
  reset() {
    this.speed = 5
  }
  addSpeed() {
    this.speed = this.speed + this.stage
    if (this.speed >= this.max) this.speed = this.max
  }
  subSpeed() {
    this.speed = this.speed - this.stage
    if (this.speed <= 1) this.speed = 1
  }
  setSpeed(speed){
    if(typeof speed !== 'number') return console.error("speed value is not a number")
    this.speed = Math.max(1,Math.min(speed,this.max))
  }
  toRight() {
    this.clear()
    this.direction = 'right'
    this.start()
  }
  toLeft() {
    this.clear()
    this.direction = 'left'
    this.start()
  }
  clear() {
    clearInterval(this.timer)
    this.timer = null
  }
}

export default {
  name: 'MoveImage',
  props: {
    options: {
      type: Object,
      default: function () {
        return {
          autoplay: true,
          width: '560px',
          height: '484px',
        }
      },
    },
    imgs: {
      type: Array,
      default: function () {
        return []
      },
    },
  },
  data() {
    return {
      moveImg: null,
    }
  },
  mounted() {
    this.moveImg = new MoveImage(this.$refs.imgsBox, this.options)
    if (this.options.autoplay) {
      this.moveImg.start()
    }
  },
  methods: {
    start() {
      this.moveImg.start()
    },
    stop() {
      this.moveImg.stop()
    },
    setSpeed(speed){
      this.moveImg.setSpeed(speed)
    },
    addSpeed() {
      this.moveImg.addSpeed()
    },
    subSpeed() {
      this.moveImg.subSpeed()
    },
    toLeft() {
      this.moveImg.toLeft()
    },
    toRight() {
      this.moveImg.toRight()
    },
    reset() {
      this.moveImg.reset()
    },
    randomColor() {
      return `rgb(${Math.floor(Math.random() * 256)}, ${Math.floor(
        Math.random() * 256
      )}, ${Math.floor(Math.random() * 256)})`
    },
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.container {
  width: 100%;
}

/**
  width: 100vw;
*/
.box_img_box {
  overflow: hidden;
  position: relative;
}
.imgs_box {
  display: flex;
  justify-content: flex-start;
  align-items: flex-start;
  // 获取真实宽度
  position: absolute;
}
.box_img_box .img_item {
  width: 500px;
  height: 300px;
}
.rect {
  width: 100vw;
  height: 280px;
  &.rect_0 {
    background: #000;
  }
  &.rect_1 {
    background: #666;
  }
  &.rect_2 {
    background: pink;
  }
}
</style>
