<script setup lang="ts">
import p5 from 'p5'

const sketch = (p5: p5) => {
  function Particle(x, y) {
    this.x = x
    this.y = y
    this.pos = p5.createVector(this.x, this.y)

    this.life = p5.random(1)
    this.c = p5.color(p5.random(colors))
    this.ff = 0

    this.update = function () {
      this.ff = p5.noise(this.pos.x / 100, this.pos.y / 100) * p5.TWO_PI // Flow Field
      let mainP = 1200
      let changeDir = p5.TWO_PI / mainP
      let roundff = p5.round((this.ff / p5.TWO_PI) * mainP) // round ff
      // *** main point *** //
      this.ff = changeDir * roundff

      if (this.ff < 6 && this.ff > 3) {
        this.c = colors[0]
        p5.stroke(this.c)
        this.pos.add(p5.tan(this.ff) * p5.random(1, 3), p5.tan(this.ff))
      } else {
        this.c = colors[1]
        p5.stroke(this.c)
        this.pos.sub(p5.sin(this.ff) * p5.random(0.1, 1), p5.cos(this.ff))
      }
    }

    this.show = function () {
      p5.noFill()
      p5.strokeWeight(p5.random(1.25))
      let lx = 20
      let ly = 20
      let px = p5.constrain(this.pos.x, lx, p5.width - lx)
      let py = p5.constrain(this.pos.y, ly, p5.height - ly)
      p5.point(px, py)
    }

    this.finished = function () {
      this.life -= p5.random(p5.random(p5.random(p5.random()))) / 10
      this.life = p5.constrain(this.life, 0, 1)
      if (this.life == 0) {
        return true
      } else {
        return false
      }
    }
  }

  let particles = []
  let colors = []
  let parNum = 1000
  let mySize

  p5.setup = () => {
    mySize = p5.min(window.innerWidth, window.innerHeight)
    p5.pixelDensity(5)
    p5.createCanvas(window.innerWidth, window.innerHeight)
    p5.colorMode(p5.HSB, 360, 100, 100, 100)
    colors[0] = p5.color(90)
    colors[1] = p5.color(80)
    for (let i = 0; i < parNum; i++) {
      particles.push(new Particle(p5.random(p5.width), p5.random(p5.height)))
    }
    p5.background(255, 255, 255, 0)
  }
  p5.draw = () => {
    for (let j = particles.length - 1; j > 0; j--) {
      particles[j].update()
      particles[j].show()
      if (particles[j].finished()) {
        particles.splice(j, 1)
        p5.background(0, 0, 0, 0)
      }
    }
    for (let i = particles.length; i < parNum; i++) {
      particles.push(new Particle(p5.random(p5.width), p5.random(p5.height)))
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
