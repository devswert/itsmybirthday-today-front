<template>
  <div class="row flex-center">
    <confetti></confetti>
    <div class="xs-12 sm-8 md-7" style="z-index: 2;">
      <div class="paper text-center">
        <img src="../assets/logo.png" width="150" class="no-border no-responsive">
        <h1>WTF?</h1>
        <div class="section">
          <article class="article">
            <p class="article-meta">
              Coding by <a href="https://github.com/devswert" target="_blank">Leonardo David</a> and <a href="https://github.com/eaperezej" target="_blank">Ernesto Perez</a> when have lot of work but this guys are stupid ¯\_(ツ)_/¯
            </p>

            <p>
              This project is stupid but beautiful, inspired for the great site <a href="https://isitchristmas.com/" target="_blank">Is It Christmas?</a>
            </p>

            <p>
              Our speacial thanks to:
              <ul>
                <li><a href="https://github.com/papercss/papercss" target="_blank">Paper CSS</a> for make a great UI</li>
                <li><a href="https://codepen.io/jonathanbell/pen/OvYVYw" target="_blank">This guy</a> how make a confetti animation and we adapted to Vue</li>
              </ul>
            </p>
          </article>
        </div>
        <div class="row flex-edges">
          <router-link class="paper-btn" :to="{ name: 'Home' }">Go Back</router-link>
          <a target="_blank" href="https://github.com/devswert/itsmybirthday-today-front" class="paper-btn btn-primary">See on GitHub</a>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'About',
  data () {
    return {}
  },
  methods: {}
}
</script>
