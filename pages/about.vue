<iframe class="aligncenter" src="https://player.vimeo.com/video/534832934" width="640" height="360" frameborder="0" allow="autoplay; fullscreen; picture-in-picture" allowfullscreen></iframe>    
<template>
  <v-row class="mt-5" justify="center" align="center">
    <v-col cols="12" sm="8" md="6">
      <div class="text-center">
        <iframe class="aligncenter" src="https://player.vimeo.com/video/534832934" width="640" height="360" frameborder="0" allow="autoplay; fullscreen; picture-in-picture" allowfullscreen>
          Lorem, ipsum dolor sit amet consectetur adipisicing elit. Aut doloremque
          fugiat dolorem eligendi consequatur quas ab, ducimus voluptatum iure
          mollitia modi, dolorum delectus accusamus aspernatur ipsa temporibus
          libero, pariatur sapiente?
        </iframe>
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
