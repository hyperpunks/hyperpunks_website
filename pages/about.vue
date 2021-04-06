<template>
  <v-row class="mt-5" justify="center" align="center">
    <v-col cols="12" sm="8" md="6">
      <div class="text-center">
        <!-- SHOW A COUNTDOWN TIMER -->
        <v-container v-if="currentTime" class="mt-5">
          <p style="margin: 50px" class="text-4xl redtext mb-5">
            <img src="/logo.png" alt="hyperpunks logo" />
          </p>
          <v-row>
            <v-col>
              <v-card style="font-size: 3em" elevation="0">
                {{ currentTime.days }}
              </v-card>
              <v-card style="color: #560503" elevation="0"> Days </v-card>
            </v-col>
            <v-col>
              <v-card style="font-size: 3em" elevation="0">
                {{ ('0' + currentTime.hours).slice(-2) }}
              </v-card>
              <v-card style="color: #560503" elevation="0"> Hours </v-card>
            </v-col>
            <v-col>
              <v-card style="font-size: 3em" elevation="0">
                {{ ('0' + currentTime.minutes).slice(-2) }}
              </v-card>
              <v-card style="color: #560503" elevation="0"> Minutes </v-card>
            </v-col>
            <v-col>
              <v-card style="font-size: 3em" elevation="0">
                {{ ('0' + currentTime.seconds).slice(-2) }}
              </v-card>
              <v-card style="color: #560503" elevation="0"> Seconds </v-card>
            </v-col>
          </v-row>
        </v-container>

        <!-- SHOW A COUNTDOWN TIMER -->
        <section v-if="!currentTime">
          <img src="/logo.png" style="size: 150px" alt="hyperpunks logo" />
          <v-spacer />

          <a href="#">AR</a>
        </section>
      </div>
    </v-col>
  </v-row>
</template>

<script>
export default {
  auth: false,
  data() {
    return {
      currentTime: null,
      search: '',
      deadline: '01/01/2021',
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
      const t = Date.parse(this.deadline) - Date.parse(new Date())
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
        this.currentTime = null
      }
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
