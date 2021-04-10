<template>
  <v-container>
    <v-row justify="center" align="center">
      <v-col cols="6">
        <iframe
          class="aligncenter"
          src="https://player.vimeo.com/video/534823039"
          width="640"
          height="296"
          frameborder="0"
          allow="autoplay; fullscreen; picture-in-picture"
          allowfullscreen
        ></iframe>
        <div class="ma-5 headline text-center" align="center">
          Augmented Reality
        </div>
        <div class="ma-5 darkred text-center">
          Coming Soon. Get read for seeing your punks in the real world...
        </div>
        <div class="ma-5 darkred text-center">
          *We do not retain any royalties from the resale of the NFTs.
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
