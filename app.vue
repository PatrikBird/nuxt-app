<script setup lang="ts">
import p5 from "p5"

const sketch = (p5: p5) => {
  p5.setup = () => {
    p5.createCanvas(window.innerWidth, window.innerHeight)
    p5.angleMode(p5.DEGREES)
  }
  p5.draw = () => {
    p5.background(20)
    p5.strokeWeight(2)
    p5.noFill()

    p5.translate(p5.width / 2, p5.height / 2)

    for (var t = 0; t < 5; t++) {
      p5.stroke(100 - t * 20, 150 - t * 30, 220 - t * 30)
      p5.beginShape()
      for (var i = 0; i < 359; i++) {
        var r1Min = p5.map(p5.sin(p5.frameCount), -1, 1, 50, 120)
        var r1Max = p5.map(p5.sin(p5.frameCount * 2), -1, 1, 100, 20)
    
        var r2Min = p5.map(p5.sin(p5.frameCount / 2), -1, 1, 120, 50)
        var r2Max = p5.map(p5.sin(p5.frameCount), -1, 1, 20, 100)
    
        var r1 = p5.map(p5.sin(i * 3), -1, 1, r1Min, r1Max)
        var r2 = p5.map(p5.sin(i * 6 + 90), -1, 1, r2Min, r2Max)
        var r = r1 + r2 - t * 10
        var x = r * p5.cos(i)
        var y = r * p5.sin(i)
        p5.vertex(x, y)
      }
      p5.endShape(p5.CLOSE)
    }
  }
}
</script>

<template>
  <div>
    <P5Wrapper :sketch="sketch" />
    <NuxtWelcome />
  </div>
</template>
