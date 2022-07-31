<script setup lang="ts">
import p5 from 'p5'

const sketch = (p5: p5) => {
  let yoff = 0.0 // 2nd dimension of perlin noise
  p5.setup = () => {
    p5.createCanvas(window.innerWidth, window.innerHeight)
  }
  p5.draw = () => {
    p5.background(245, 245, 245)
    p5.fill(51)
    p5.beginShape()

    let xoff = 0
    for (let x = 0; x <= p5.width; x += 10) {
      let y = p5.map(p5.noise(xoff, yoff), 0, 1, 500, 300)
      p5.vertex(x, y)
      xoff += 0.05
    }

    // increment y dimension for noise
    yoff += 0.01
    p5.vertex(p5.width, p5.height)
    p5.vertex(0, p5.height)
    p5.endShape(p5.CLOSE)
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
