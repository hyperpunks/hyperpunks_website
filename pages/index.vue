<template>
  <v-container v-if="isLoaded">
    <form class="search-form" @submit.prevent="searchForToken">
      <div class="search-form__row">
        <v-text-field
          v-model="tokenID"
          class="pt-5 ma-1 form-input"
          placeholder="*Search a number between 0 - 9999"
          solo
        ></v-text-field>
        <v-btn
          style="max-height: 48px"
          color="#450302"
          x-large
          class="mb-2"
          input
          @click="searchForToken()"
        >
          GO
        </v-btn>
      </div>
    </form>
  </v-container>
</template>

<script>
import { DEADLINE } from '../constants'
export default {
  auth: false,
  data() {
    return {
      tokenID: '',
      countdownFinished: false,
      isLoaded: false,
    }
  },
  mounted() {
    const t = Date.parse(DEADLINE) - Date.parse(new Date())
    if (t > 0) {
      this.countdownFinished = true
      this.$router.push('/countdown')
      return
    }
    this.isLoaded = true
  },
  methods: {
    searchForToken() {
      this.$router.push('/nft?id=' + this.tokenID)
    },
  },
}
</script>

<style lang="scss" scoped>
.container {
  max-width: 1500px;
}
.black-text {
  color: black i !important;
}

.theme--dark.v-input input,
.theme--dark.v-input textarea {
  color: #ea201c;
}
</style>
