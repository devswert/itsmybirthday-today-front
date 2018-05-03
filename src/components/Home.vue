<template>
  <div class="row flex-center">
    <confetti></confetti>
    <div class="xs-12 sm-8 md-7" style="z-index: 2;">
      <!-- Enter Date -->
      <div class="paper text-center" v-if="waitingDate">
        <img src="../assets/logo.png" width="250" class="no-border no-responsive">
        <h2>Its my Birthday today?</h2>
        <div class="form-group">
          <label for="birthday-ipnut">When do you born?</label>
          <input type="date" v-model="birthday" placeholder="Enter a date" id="birthday-ipnut" class="input-block">
          <br>
          <button type="button" class="btn-primary" @click="happy()">Validate</button>
        </div>
      </div>

      <!-- Success -->
      <div class="paper text-center" v-if="!waitingDate && itsYourBirthday">
        <img src="../assets/birthday.png" width="250" class="no-border no-responsive">
        <h2 class="text-success" v-if="years > 0">Happy {{ years }} Birthday!</h2>
        <h2 class="text-success" v-if="years === 0">You born today!</h2>
        <button type="button" class="btn-primary" @click="enterNewDate()">Back</button>
      </div>

      <!-- Not Yet :C -->
      <div class="paper text-center" v-if="!waitingDate && !itsYourBirthday">
        <img :src="source" width="250" class="no-border no-responsive">
        <h2 class="text-danger">Sorry, today isn't your birthday :(</h2>
        <button type="button" class="btn-primary" @click="enterNewDate()">Back</button>
      </div>

      <div class="text-right">
        <router-link :to="{ name: 'About' }">About this</router-link>
      </div>
    </div>
  </div>
</template>

<script>
import moment from 'moment'
import Confetti from '@/components/Confetti'

export default {
  name: 'Home',
  components: {
    'confetti': Confetti
  },
  data () {
    return {
      years: 0,
      source: null,
      birthday: null,
      waitingDate: true,
      itsYourBirthday: false
    }
  },
  methods: {
    itsMyBirthday () {
      // This State
      this.waitingDate = false
      this.itsYourBirthday = false
      this.source = '/static/' + ((Math.floor(Math.random() * 10) % 2 === 0) ? 'cat' : 'dog') + '.png'

      // TMP
      let isValidBirthday = false
      let today = moment().format('MM-DD')
      let input = moment(this.birthday).format('MM-DD')

      if (today === input) {
        isValidBirthday = true
        this.itsYourBirthday = true
        this.years = moment().format('YYYY') - moment(this.birthday).format('YYYY')
      }

      return isValidBirthday
    },
    enterNewDate () {
      this.itsYourBirthday = false
      this.waitingDate = true
      this.birthday = null
    },
    happy () {
      if (this.itsMyBirthday()) {
        this.$children[0].throwConfetti()
      }
    }
  }
}
</script>
