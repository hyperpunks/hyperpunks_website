<template>
  <v-app dark>
    <v-app-bar v-if="!countdownFinished" fixed clipped-left app elevation="0">
      <v-toolbar-title class="ml-0 pl-4">
        <nuxt-link style="text-decoration: none" to="/">
          <img src="/logo.png" alt="hyperpunks logo" />
        </nuxt-link>
      </v-toolbar-title>
      <div class="flex-grow-1"></div>
      <v-btn to="/" class="ma-2" outlined>home</v-btn>
      <v-btn to="/about" class="ma-2" outlined>about</v-btn>
      <v-btn to="/ar" class="ma-2" outlined>ar</v-btn>
    </v-app-bar>

    <v-main>
      <nuxt />
    </v-main>
    <v-footer :fixed="fixed" app>
      <v-card class="flex" flat tile>
        <v-card-text
          v-if="showNonMainnetWarning"
          class="red white--text text-center"
        >
          <strong>Warning!</strong> Not connected to Ethereum Mainnet (network
          ID: {{ $store.state.networkID }})
        </v-card-text>

        <v-card-text class="py-2 text-center">
          {{ new Date().getFullYear() }} — <strong>Your Company</strong>
        </v-card-text>
      </v-card>
    </v-footer>
  </v-app>
</template>
<script>
import { DEADLINE } from '../constants'
export default {
  auth: false,
  data() {
    return {
      countdownFinished: false,
    }
  },
  mounted() {
    const t = Date.parse(DEADLINE) - Date.parse(new Date())
    if (t > 0) {
      this.countdownFinished = true
      this.$router.push('/countdown')
    }
  },
  methods: {},
}
</script>
<style scoped>
#styled-input {
  height: 48px;
  width: 48px;
}
.styled-input label[for] {
  height: 48px;
}
.v-navigation-drawer > .list:not(.list--dense) .list__tile {
  font-size: 17px;
}
.avatar {
  max-width: 75px;
}
/* .list__tile--active.list__tile.list__tile--link {
} */
a.nuxt-link-exact-active.list__tile--active.list__tile.list__tile--link {
  font-weight: 900 !important;
}
.v-list-item {
  border-left: 10px solid transparent;
}
.v-list-item--active {
  color: #333;
  border-left: 10px solid #ff5722;
}
</style>
