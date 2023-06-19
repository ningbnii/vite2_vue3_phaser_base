<template>
  <div class="control">
    <van-button type="primary" :to="{ path: '/index/index' }">跳转</van-button>
  </div>
  <div class="canvas_box" ref="canvasBox">
    <div ref="myCanvas"></div>
  </div>
</template>
<script setup>
import { ref, onMounted } from 'vue'
import * as Phaser from 'Phaser'
let myCanvas = ref(null)
let canvasBox = ref(null)
onMounted(() => {
  var config = {
    type: Phaser.AUTO,
    width: canvasBox.value.clientWidth,
    height: canvasBox.value.clientHeight,
    parent: myCanvas.value,
    physics: {
      default: 'arcade',
      arcade: {
        gravity: { y: 200 },
      },
    },
    scene: {
      preload: preload,
      create: create,
    },
  }

  var game = new Phaser.Game(config)

  function preload() {
    // this.load.setBaseURL("http://labs.phaser.io");

    this.load.image('sky', '/src/assets/space3.png')
    this.load.image('logo', '/src/assets/phaser3-logo.png')
    this.load.image('red', '/src/assets/red.png')
  }

  function create() {
    this.add.image(400, 300, 'sky')

    var particles = this.add.particles('red')

    var emitter = particles.createEmitter({
      speed: 100,
      scale: { start: 1, end: 0 }, // 缩放
      blendMode: 'ADD', // 混合模式
    })

    var logo = this.physics.add.image(400, 100, 'logo')

    // 解释一下，这里的logo是一个物理对象，所以可以设置物理属性，比如重力，弹性等等
    // setVelocity(x, y) 设置速度
    logo.setVelocity(100, 200)
    // setBounce(x, y) 设置弹性
    logo.setBounce(1, 1)
    // setCollideWorldBounds() 设置碰撞世界边界
    logo.setCollideWorldBounds(true)

    // 跟随logo
    emitter.startFollow(logo)
  }
})
</script>
<style lang="less" scoped>
.control {
  position: absolute;
}
.canvas_box {
  width: 100vw;
  height: 100vh;
}
</style>
