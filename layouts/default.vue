<template>
  <v-app dark>
    <v-app-bar
      v-if="!countdownFinished"
      color="transparent"
      fixed
      clipped-left
      app
      elevation="0"
    >
      <v-toolbar-title>
        <nuxt-link style="text-decoration: none" to="/">
          <img
            src="/logo.png"
            class="mt-5"
            style="height: 60px"
            alt="hyperpunks logo"
          />
        </nuxt-link>
      </v-toolbar-title>
      <div class="flex-grow-1"></div>
      <v-btn to="/about" class="ma-2" text>about</v-btn>
      <v-btn to="/ar" class="ma-2" text>ar</v-btn>
      <v-btn
        href="https://twitter.com/HyperPunks_NFT"
        target="_blank"
        fab
        text
        redtext
      >
        <v-icon>mdi-twitter</v-icon>
      </v-btn>

      <v-btn class="ma-2" text @click="metamaskButtonClicked()">{{
        walletBtnText
      }}</v-btn>
    </v-app-bar>

    <v-main>
      <nuxt />
    </v-main>
    <v-footer
      v-if="showNonMainnetWarning"
      :fixed="fixed"
      color="transparent"
      app
    >
      <v-card class="flex" flat>
        <v-card-text class="red white--text text-center">
          <strong>Warning!</strong> Not connected to Ethereum Mainnet (network
          ID: {{ $store.state.networkID }})
        </v-card-text>
      </v-card>
    </v-footer>
  </v-app>
</template>
<script>
import { ethers } from 'ethers'
import { DEADLINE } from '../constants'
export default {
  auth: false,
  data() {
    return {
      walletBtnText: 'CONNECT WALLET',
      ethers: null,
      showNonMainnetWarning: false,
      countdownFinished: false,
    }
  },
  mounted() {
    const t = Date.parse(DEADLINE) - Date.parse(new Date())
    if (t > 0) {
      this.countdownFinished = false // todo make true!
      // this.$router.push('/countdown')
      console.log('uncomment this!')
    }
  },
  methods: {
    goToUrl(url) {
      this.$router.push(url)
    },
    goToExternalUrl(url) {
      window.open(url, '_blank')
    },
    async metamaskButtonClicked() {
      if (window.ethereum) {
        await window.ethereum.enable()
        this.ethers = new ethers.providers.Web3Provider(window.ethereum)

        this.signer = this.ethers.getSigner()
        this.account = await this.signer.getAddress()
        this.balance = await this.signer.getBalance()
        this.ethBalance = await ethers.utils.formatEther(this.balance)
        this.signer = this.ethers.getSigner()
        const addr = await this.signer.getAddress()
        this.walletBtnText =
          addr.substr(0, 7) + '...' + addr.substr(addr.length - 5, addr.length)

        const chainId = this.ethers._network.chainId
        this.$store.commit('setSelectedAddress', addr)
        this.$store.commit('setNetworkID', Number(chainId))

        if (chainId !== 1) {
          this.showNonMainnetWarning = true
        }
      } else {
        this.$router.push('/other/install_metamask')
      }
    },
  },
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
