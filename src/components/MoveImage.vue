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
class MoveImage {
  constructor(imgsBox, options = {}) {
    this.options = options
    // 获取图片容器
    this.imgsBox = imgsBox
    // 获取图片宽
    this.imgWidth = this.imgsBox.clientWidth / this.imgsBox.children.length
    // 初始速度
    this.speed = options.speed || 5
    // 添加移动方向
    this.direction = options.direction || 'left'
    // 初始化加减步骤大小
    this.stage = options.stage || 1
    // 初始化最大速度
    this.max = options.max || 15
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
    this.translate(-this.imgWidth)
  }
  start() {
    if (this.timer) {
      return
    }
    this.timer = setInterval(() => {
      //  获取当前left
      let l = this.imgsBox.offsetLeft
      if (this.direction === 'right') l += this.speed
      else l -= this.speed
      // 判断是否到第一张或最后一张

      if (l <= -(this.imgsBox.children.length - 1) * this.imgWidth) {
        this.translate(-this.imgWidth)
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
    }, 20)
  }
  translate(left) {
    this.imgsBox.style.left = left + 'px'
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
