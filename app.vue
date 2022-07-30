<script setup lang="ts">
import p5 from 'p5'

const sketch = (p5: p5) => {
  let points = []
  p5.setup = () => {
    p5.createCanvas(window.innerWidth, window.innerHeight)
    p5.pixelDensity(2)
    p5.angleMode(p5.DEGREES)
    p5.noiseDetail(1, 1)
    let density = 100

    for (var x = 0; x <= p5.width; x += p5.width / density) {
      for (var y = 0; y <= p5.height; y += p5.width / density) {
        var p = p5.createVector(x + p5.random(-10, 10), y + p5.random(-10, 10))
        points.push(p)
      }
    }
    p5.background(245, 245, 245)
  }
  p5.draw = () => {
    if (p5.frameCount < 400) {
      p5.noStroke()
      var mult = 0.01
      for (var j = 0; j < 1; j++) {
        for (var i = 0; i < points.length; i++) {
          var r = p5.map(points[i].x, 0, p5.width, 50, 255)
          var g = p5.map(points[i].y, 0, p5.height, 255, 50)
          var b = p5.map(points[i].x, 0, p5.width, 255, 50)
          p5.fill(r, g, b, 10)
          var angle = p5.map(
            p5.noise(points[i].x * mult, points[i].y * mult),
            0,
            1,
            0,
            720
          )
          points[i].add(p5.createVector(p5.cos(angle), p5.sin(angle)))
          p5.ellipse(points[i].x, points[i].y, 1)
        }
      }
    } else {
      p5.noLoop()
    }
  }
}
</script>

<template>
  <div>
    <P5Wrapper class="test" :sketch="sketch" />
    <div>
      <h1>Hi :)</h1>
    </div>
  </div>
</template>

<style>
.test {
  position: absolute;
  left: 0;
  top: 0;
  z-index: -1;
}
</style>
