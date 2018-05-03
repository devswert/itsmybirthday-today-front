<template>
  <canvas id="confetti" class="confetti-container"></canvas>
</template>

<script>
export default {
  name: 'Confetti',
  data () {
    return {
      w: 0,
      h: 0,
      colors: [
        'DodgerBlue',
        'OliveDrab',
        'Gold',
        'Pink',
        'SlateBlue',
        'LightBlue',
        'Gold',
        'Violet',
        'PaleGreen',
        'SteelBlue',
        'SandyBrown',
        'Chocolate',
        'Crimson'
      ],
      canvas: null,
      context: null,
      particles: [],
      max_confettis: 50,
      animation_id: null,
      particles_removed: 0,
      confetti_animation: false
    }
  },
  methods: {
    ConfettiParticle (vm) {
      this.x = Math.random() * vm.w // x
      this.y = Math.random() * vm.h - vm.h // y
      this.r = vm.randomFromTo(11, 33) // radius
      this.d = Math.random() * vm.max_confettis + 11
      this.color = vm.colors[Math.floor(Math.random() * vm.colors.length)]
      this.tilt = Math.floor(Math.random() * 33) - 11
      this.tiltAngleIncremental = Math.random() * 0.07 + 0.05
      this.tiltAngle = 0
      this.cycle = 1
      this.times = 0
      this.can_animate = true

      this.draw = function () {
        vm.context.beginPath()
        vm.context.lineWidth = this.r / 2
        vm.context.strokeStyle = this.color
        vm.context.moveTo(this.x + this.tilt + this.r / 3, this.y)
        vm.context.lineTo(this.x + this.tilt, this.y + this.tilt + this.r / 5)
        return vm.context.stroke()
      }
    },
    throwConfetti () {
      if (!this.confetti_animation) {
        this.canvas.width = this.w
        this.canvas.height = this.h
        this.confetti_animation = true
        this.draw()
      }
    },
    randomFromTo (from, to) {
      return Math.floor(Math.random() * (to - from + 1) + from)
    },
    draw () {
      let results = []

      // Magical recursive functional love
      this.animation_id = requestAnimationFrame(this.draw)

      this.context.clearRect(0, 0, this.w, window.innerHeight)

      for (let i = 0; i < this.max_confettis; i++) {
        results.push(this.particles[i].draw())
      }

      let particle = {}
      // let remainingFlakes = 0
      for (let i = 0; i < this.max_confettis; i++) {
        particle = this.particles[i]

        if (particle.can_animate) {
          particle.tiltAngle += particle.tiltAngleIncremental
          particle.y += (Math.cos(particle.d) + 3 + particle.r / 2) / 2
          particle.tilt = Math.sin(particle.tiltAngle - i / 3) * 15

          // if (particle.y <= this.h) remainingFlakes++

          // If a confetti has fluttered out of view,
          // bring it back to above the viewport and let if re-fall.
          if (particle.x > this.w + 30 || particle.x < -30 || particle.y > this.h) {
            particle.x = Math.random() * this.w
            particle.y = -30
            particle.tilt = Math.floor(Math.random() * 10) - 20

            if (particle.cycle <= particle.times) {
              this.removeParticle(particle)
            } else {
              particle.times++
            }
          }
        }
      }

      return results
    },
    removeParticle (particle) {
      this.particles_removed++
      particle.can_animate = false

      if (this.particles_removed === this.max_confettis) {
        cancelAnimationFrame(this.animation_id)
        this.clearConfettiAnimations()
      }
    },
    clearConfettiAnimations () {
      this.particles_removed = 0
      this.confetti_animation = false
      for (let i = 0; i < this.max_confettis; i++) {
        this.particles[i].times = 0
        this.particles[i].can_animate = true
      }
    }
  },
  mounted () {
    this.w = window.innerWidth
    this.h = window.innerHeight
    this.canvas = document.getElementById('confetti')
    this.context = this.canvas.getContext('2d')

    // Push new confetti objects to `particles[]`
    for (let i = 0; i < this.max_confettis; i++) {
      this.particles.push(new this.ConfettiParticle(this))
    }
  }
}
</script>
