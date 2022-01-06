<template>
  <div class="control">
    <van-button type="primary" :to="{ path: '/index/index' }">跳转</van-button>
  </div>
  <div class="canvas_box" ref="canvasBox">
    <div ref="myCanvas"></div>
  </div>
</template>
<script setup>
import { ref, onMounted } from "vue";
import * as Phaser from "Phaser";
let myCanvas = ref(null);
let canvasBox = ref(null);
onMounted(() => {
  var config = {
    type: Phaser.AUTO,
    width: canvasBox.value.clientWidth,
    height: canvasBox.value.clientHeight,
    parent: myCanvas.value,
    physics: {
      default: "arcade",
      arcade: {
        gravity: { y: 200 },
      },
    },
    scene: {
      preload: preload,
      create: create,
    },
  };

  var game = new Phaser.Game(config);

  function preload() {
    // this.load.setBaseURL("http://labs.phaser.io");

    this.load.image("sky", "/src/assets/space3.png");
    this.load.image("logo", "/src/assets/phaser3-logo.png");
    this.load.image("red", "/src/assets/red.png");
  }

  function create() {
    this.add.image(400, 300, "sky");

    var particles = this.add.particles("red");

    var emitter = particles.createEmitter({
      speed: 100,
      scale: { start: 1, end: 0 },
      blendMode: "ADD",
    });

    var logo = this.physics.add.image(400, 100, "logo");

    logo.setVelocity(100, 200);
    logo.setBounce(1, 1);
    logo.setCollideWorldBounds(true);

    emitter.startFollow(logo);
  }
});
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
