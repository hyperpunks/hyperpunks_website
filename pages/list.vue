<template>
  <v-container v-if="isLoaded">
    <div class="items-container">
      <div v-for="item in items" class="single-item" :key="item.id">
        <nuxt-link :class="item.is_sold === true ? 'sold' : ''" :to="item.url">
          {{ item.id }}</nuxt-link
        >
      </div>
      <div class="loading" v-if="loading && text">{{ text }}</div>
    </div>
  </v-container>
</template>

<script>
import { ethers } from 'ethers'
import { CONTRACT_ADDR, RPC_PROVIDER, NETWORK_ID } from '../constants'
import { ERC1155_ABI } from '../erc1155_abi'

export default {
  auth: false,
  data() {
    return {
      items: [],
      isLoaded: false,
      loading: true,
      page: 1,
      contract: null,
      itemsCurrentNumber: 0,
      batchSize: 250,
      pagesMaxNumber: 40,
      text: 'loading...',
      ethers: null,
      provider: null,
    }
  },
  mounted() {
    this.contractAddress = CONTRACT_ADDR
    if (!window.ethereum) {
      this.provider = 'not_web3'
      this.ethers = new ethers.providers.JsonRpcProvider(
        RPC_PROVIDER,
        NETWORK_ID
      )
    } else {
      this.provider = 'web3'
      this.ethers = new ethers.providers.Web3Provider(window.ethereum)
    }
    this.contract = new ethers.Contract(CONTRACT_ADDR, ERC1155_ABI, this.ethers)
    this.getInitialData()
    this.getNextData()
    this.isLoaded = true
  },
  beforeMount() {
    //mmm
  },
  methods: {
    getInitialData() {
      this.loadData()
    },
    getNextData() {
      window.onscroll = () => {
        const bottomOfWindow =
          document.documentElement.scrollTop + window.innerHeight ===
          document.documentElement.offsetHeight
        if (bottomOfWindow) {
          this.loadData()
        }
      }
    },
    loadData() {
      this.loading = true
      if (this.page === this.pagesMaxNumber) {
        this.text = ''
        return
      }
      const arr = [].concat(this.items)
      for (
        let i = (this.page - 1) * this.batchSize,
          l = this.page * this.batchSize;
        i < l;
        i++
      ) {
        const item = {
          id: i,
          is_sold: false,
          url: '/nft?id=' + i,
        }
        arr.push(item)
        this.checkIsSold(i)
      }
      this.items = [].concat(arr)
      this.page++
      setTimeout(function () {
        this.loading = false
      }, 1000)
    },
    async checkIsSold(itemId) {
      const tokenSupply = await this.contract.tokenSupply(itemId)
      console.log(Number(tokenSupply))
      if (Number(tokenSupply) !== 0) {
        const filtered = this.items.filter(this.compareId(itemId))
        console.log(filtered)
        if (filtered.length) {
          filtered[0].is_sold = true
        }
      }
    },
    compareId(id) {
      return function (element) {
        return element.id === id
      }
    },
  },
}
</script>

<style lang="scss" scoped>
.container {
  max-width: 1500px;
  text-align: center;
  .items-container {
    max-width: 800px;
    display: inline-block;
    text-align: left;
    width: 100%;
    /*.v2-lazy-list-wrap {
      height: 600px !important;
    }*/
    /*.lazy-list-item {
      height: auto !important;
    }*/
    .single-item {
      display: inline-block;
      margin: 11px;
      a.sold {
        color: #661515 !important;
      }
    }
  }
}
.black-text {
  color: black !important;
}

.lazy-list-item {
  height: auto !important;
}

.theme--dark.v-input input,
.theme--dark.v-input textarea {
  color: #ea201c;
}
</style>
