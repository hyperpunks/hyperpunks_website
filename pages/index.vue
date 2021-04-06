<template>
  <v-row class="mt-5" justify="center" align="center">
    <v-col cols="12" sm="8" md="6">
      <div class="text-center">index page</div>
    </v-col>
  </v-row>
</template>

<script>
import { DEADLINE } from '../constants'
export default {
  auth: false,
  data() {
    return {
      search: '',
    }
  },
  mounted() {
    const t = Date.parse(DEADLINE) - Date.parse(new Date())
    if (t > 0) {
      this.countdownFinished = true
      this.$router.push('/countdown')
    }
  },
  methods: {
    countdown() {},
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
