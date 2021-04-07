<template>
  <v-layout align-center justify-center>
    <v-flex v-if="nft" xs12 sm8 md6 ma-5 style="max-width: 600px">
      <v-card elevation="0" class="pa-5">
        <v-card-title>{{ nft.name }}</v-card-title>

        <v-img
          v-if="!nft.animation_url"
          :src="nft.image"
          max-height="600"
          contain
        >
        </v-img>

        <model-viewer
          v-if="nft.animation_url"
          data-js-focus-visible
          style="width: 600px; height: 600px"
          :src="nft.animation_url"
          :alt="nft.description"
          auto-rotate
          camera-controls
          preload
        ></model-viewer>

        <v-card-text>
          {{ nft.description }}
        </v-card-text>

        <v-card-text>
          <v-list>
            <v-list-item v-for="(item, i) in nft.attributes" :key="i">
              <v-list-item-content>
                <v-list-item-title v-text="item.trait_type"></v-list-item-title>
                <v-list-item-subtitle
                  v-text="item.value"
                ></v-list-item-subtitle>
              </v-list-item-content>
            </v-list-item>
          </v-list>
        </v-card-text>

        <v-card-text>
          <a target="_blank" :href="nft.external_url">{{ nft.external_url }}</a>
        </v-card-text>

        <v-card-text>
          <v-list dense>
            <v-list-group
              v-for="item in infoItems"
              :key="item.title"
              v-model="item.active"
              :prepend-icon="item.action"
              no-action
            >
              <template #activator>
                <v-list-item-content>
                  <v-list-item-title v-text="item.title"></v-list-item-title>
                </v-list-item-content>
              </template>

              <v-list-item v-for="child in item.items" :key="child.title">
                <v-list-item-content>
                  <v-list-item-subtitle
                    v-text="child.title"
                  ></v-list-item-subtitle>
                </v-list-item-content>
              </v-list-item>
            </v-list-group>
          </v-list>
        </v-card-text>

        <v-card-actions v-if="itemPriceETH && !isOwned">
          <v-spacer></v-spacer>
          <v-btn width="150px" elevation="0" class="primary" @click="buyNow()"
            >buy now</v-btn
          >
          <v-icon right>mdi-ethereum</v-icon>
          <span class="body-1">{{ itemPriceETH }}</span>
        </v-card-actions>

        <v-card-actions v-if="isOwned">
          <v-spacer></v-spacer>
          <span class="title orange--text">this item is not for sale</span>
        </v-card-actions>
      </v-card>
    </v-flex>
  </v-layout>
</template>
<script>
import { mapState } from 'vuex'
import '@google/model-viewer/dist/model-viewer'
import { ethers } from 'ethers'
import { CONTRACT_ADDR } from '../../constants'
import { ERC1155_ABI } from '../../erc1155_abi'
const EthersUtils = require('ethers').utils

export default {
  auth: false,
  data() {
    return {
      id: null,
      contract: null,
      itemPriceETH: null,
      itemPriceWei: null,
      nft: null,
      isOwned: false,
      ethers: null,
      signer: null,
      infoItems: [],
    }
  },
  computed: {
    ...mapState(['selectedAddress']),
    someComputedLocalState() {
      return this.selectedAddress
    },
  },
  watch: {
    selectedAddress(val, oldVal) {
      if (val.length > 0) {
        this.ethers = new ethers.providers.Web3Provider(window.ethereum)
        this.loadContract()
      }
    },
  },

  mounted() {
    this.id = this.$route.query.id
    this.loadNFT(this.id)
    if (window.ethereum) {
      this.ethers = new ethers.providers.Web3Provider(window.ethereum)
      this.loadContract()
    } else {
      console.warn('window.ethereum NOT detected!')
    }

    this.infoItems = [
      {
        action: 'mdi-information-outline',
        items: [
          {
            title: 'Contract: ' + CONTRACT_ADDR,
          },
          {
            title: 'Token ID: ' + this.id,
          },
          {
            title: 'Blockchain: Ethereum',
          },
        ],
        title: 'Chain Info',
      },
      {
        action: 'mdi-heart',
        items: [
          {
            title:
              'Created by X. A collection of rare artwork based on X themes',
          },
          {
            title: 'Only limited copies will exist',
          },
        ],
        title: 'About Collection',
      },
    ]
  },
  methods: {
    loadNFT(id) {
      this.$axios
        .$get(
          'https://raw.githubusercontent.com/AndreiD/Playground/master/nfts_sample/json/' +
            id
        )
        .then((response) => {
          this.nft = response
        })
    },
    async loadContract() {
      this.signer = this.ethers.getSigner()
      this.contract = new ethers.Contract(
        CONTRACT_ADDR,
        ERC1155_ABI,
        this.signer
      )
      this.itemPriceWei = await this.contract.getItemPrice()
      this.itemPriceETH = EthersUtils.formatEther(this.itemPriceWei)

      const tokenSupply = await this.contract.tokenSupply(this.id)
      if (Number(tokenSupply) !== 0) {
        console.warn('this token is already owned')
        this.isOwned = true
      }
    },
    async buyNow() {
      const overrides = { value: this.itemPriceWei, gasLimit: 120000 }

      try {
        const tx = await this.contract.buyOne(this.id, overrides)
        if (tx.hash) {
          this.$toast.info('Transaction submitted successfully')
        }
      } catch (err) {
        if (err.message.includes('denied')) {
          this.$toast.info('you canceled the transaction')
        } else {
          this.$toast.error(err.message)
        }
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
</style>
