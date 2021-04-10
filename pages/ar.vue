<template>
 
  <v-row class="mt-5" justify="center" align="center">
    <v-col cols="12" sm="8" md="6">
      <iframe src="https://player.vimeo.com/video/534823039" width="640" height="296" frameborder="0" allow="autoplay; fullscreen; picture-in-picture" allowfullscreen></iframe>
       <div class="text-center">
        AR Lorem, ipsum dolor sit amet consectetur adipisicing elit. Aut
        doloremque fugiat dolorem eligendi consequatur quas ab, ducimus
        voluptatum iure mollitia modi, dolorum delectus accusamus aspernatur
        ipsa temporibus libero, pariatur
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
