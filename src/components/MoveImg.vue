<template>
  <div class="container">
    <div class="box" ref="boxRef">
      <div class="box_imgBox">
        <div class="imgsBox" ref="imgsBox">
          <img src="./方案一/背景动.png" alt="" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
class MoveImage {
  constructor(imgsBox) {
    // 获取图片容器
    this.imgsBox = imgsBox
    // 获取图片宽
    this.imgWidth = this.imgsBox.clientWidth / this.imgsBox.children.length
    // 初始速度
    this.speed = 5
    // 添加移动方向
    this.direction = 'left'
    // 添加定时器
    this.timer = null
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
    this.imgsBox.style.left = -this.imgWidth + 'px'
  }
  start() {
    if(this.timer){
      return
    }
    this.timer = setInterval(() => {
      //  获取当前left
      let l = this.imgsBox.offsetLeft
      if (this.direction === 'right') l += this.speed
      else l -= this.speed
      // 判断是否到第一张或最后一张

      if (l <= -(this.imgsBox.children.length - 1) * this.imgWidth) {
        this.imgsBox.style.left = -this.imgWidth + 'px'
        return
      } else {
        this.imgsBox.style.left = l + 'px'
      }
      if (l >= 0) {
        this.imgsBox.style.left =
          -(this.imgsBox.children.length - 2) * this.imgWidth + 'px'
        return
      } else {
        this.imgsBox.style.left = l + 'px'
      }
    }, 20)
  }
  stop() {
    clearInterval(this.timer)
    this.timer = null
  }
  reset() {
    this.speed = 5
  }
  addSpeed() {
    this.speed++
    if (this.speed >= 15) this.speed = 15
  }
  subSpeed() {
    this.speed--
    if (this.speed <= 1) this.speed = 1
  }
  toRight() {
    clearInterval(this.timer)
    this.direction = 'right'
    this.timer = null
    this.start()
  }
  toLeft() {
    clearInterval(this.timer)
    this.direction = 'left'
    this.timer = null
    this.start()
  }
}

export default {
  name: 'MoveImg',
  props: {
    config: {
      type: Object,
      default: function () {
        return {
          autoplay: true,
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
    this.moveImg = new MoveImage(this.$refs.imgsBox)
    if (this.config.autoplay) {
      // this.moveImg.start()
    }
  },
  methods: {
    start() {
      this.moveImg.start()
    },
    stop() {
      this.moveImg.stop()
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
      this.moveImg.toLeft()
    },
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.container {
  width: 100%;
}
.box {
  width: 520px;
  margin: 50px auto;
}

.box_imgBox {
  width: 500px;
  height: 280px;
  overflow: hidden;
  position: relative;
}
.imgsBox {
  display: flex;
  justify-content: flex-start;
  align-items: flex-start;
  position: absolute;
}
.box_imgBox img {
  width: 500px;
  height: 280px;
}
</style>
