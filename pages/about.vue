<template>
  <v-container>
    <v-row justify="center" align="center">
      <v-col cols="6">
        <iframe
          class="aligncenter"
          src="https://player.vimeo.com/video/534832934"
          width="640"
          height="296"
          frameborder="0"
          allow="autoplay; fullscreen; picture-in-picture"
          allowfullscreen
        ></iframe>
        <div class="ma-5 headline text-center" align="center">HyperPunks</div>
        <div class="ma-5 darkred text-center">
          10,000 unique collectible 3D NFTs with proof of ownership stored on
          the Ethereum blockchain. HyperPunks pays homage to what came before
          and pushes the boundaries of what an NFT could be. Built on the
          ERC1155 standard, compatible with all secondary marketplaces like
          OpenSea.
        </div>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  auth: false,
  data() {
    return {
      currentTime: null,
    }
  },
  mounted() {
    this.search = ''
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
