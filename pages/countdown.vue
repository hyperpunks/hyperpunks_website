<template>
  <v-row class="mt-5 countdown-row" justify="center" align="center">
    <v-col cols="12" sm="8" md="6">
      <div class="text-center">
        <!-- SHOW A COUNTDOWN TIMER -->
        <v-container v-if="currentTime" class="mt-5">
          <div class="text-4xl redtext countdown-row__logo">
            <img src="/logo.png" alt="hyperpunks logo" />
          </div>
          <v-row class="timer-row">
            <v-col>
              <v-card class="countdown-number" elevation="0">
                {{ currentTime.days }}
              </v-card>
              <v-card style="color: #560503" elevation="0"> Days </v-card>
            </v-col>
            <v-col>
              <v-card class="countdown-number" elevation="0">
                {{ ('0' + currentTime.hours).slice(-2) }}
              </v-card>
              <v-card style="color: #560503" elevation="0"> Hours </v-card>
            </v-col>
            <v-col>
              <v-card class="countdown-number" elevation="0">
                {{ ('0' + currentTime.minutes).slice(-2) }}
              </v-card>
              <v-card style="color: #560503" elevation="0"> Minutes </v-card>
            </v-col>
            <v-col>
              <v-card class="countdown-number" elevation="0">
                {{ ('0' + currentTime.seconds).slice(-2) }}
              </v-card>
              <v-card style="color: #560503" elevation="0"> Seconds </v-card>
            </v-col>
          </v-row>
        </v-container>

        <section v-if="!currentTime">
          <div class="countdown-row__logo">
            <img src="/logo.png" style="size: 150px" alt="hyperpunks logo" />
          </div>
          <br />
          <span class="countdown-row__title">time is up!</span>
        </section>

        <section style="margin-top: 50px">
          <v-btn
            href="https://twitter.com/HyperPunks_NFT"
            target="_blank"
            fab
            text
            redtext
          >
            <v-icon>mdi-twitter</v-icon>
          </v-btn>

          <v-btn
            text
            fab
            @click="goToExternalUrl('https://medium.com/@hyperpunks')"
          >
            <img src="/medium.svg" width="30" alt="hyperpunks blog" />
          </v-btn>
        </section>
      </div>
    </v-col>
  </v-row>
</template>

<script>
import { DEADLINE } from '../constants'
export default {
  auth: false,
  data() {
    return {
      currentTime: null,
      search: '',
      reports: null,
      speed: 1000,
    }
  },
  mounted() {
    setTimeout(this.countdown, 1)
    this.search = ''
  },
  methods: {
    countdown() {
      const t = Date.parse(DEADLINE) - Date.parse(new Date())
      const seconds = Math.floor((t / 1000) % 60)
      const minutes = Math.floor((t / 1000 / 60) % 60)
      const hours = Math.floor((t / (1000 * 60 * 60)) % 24)
      const days = Math.floor(t / (1000 * 60 * 60 * 24))
      if (t > 0) {
        this.currentTime = {
          total: t,
          days,
          hours,
          minutes,
          seconds,
        }
        setTimeout(this.countdown, this.speed)
      } else {
        window.location.href = 'https://hyperpunks.com'
        this.currentTime = null
      }
    },
    goToExternalUrl(url) {
      window.open(url, '_blank')
    },
  },
}
</script>

<style lang="scss" scoped>
#styled-input {
  height: 48px;
}
.styled-input label[for] {
  height: 48px;
}
.container {
  max-width: 1500px;
}
</style>
