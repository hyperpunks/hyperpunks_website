<template>
  <v-layout align-center justify-center>
    <v-flex v-if="nft" xs12 sm8 md6 ma-5 style="max-width: 900px">
      <v-row align="center" style="margin-top: 60px" justify="center">
        <v-text-field
          v-model="tokenID"
          class="pt-5 redtext ma-1"
          style="max-width: 600px"
          placeholder="*Search a number between 0 - 9999"
          solo
        ></v-text-field>
        <v-btn
          style="max-height: 48px"
          color="#450302"
          x-large
          class="mb-2"
          @click="searchForToken()"
        >
          GO
        </v-btn>
      </v-row>

      <v-card elevation="0" class="pa-5">
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

        <v-flex v-if="nft" xs12 sm8 md6 ma-5 style="max-width: 900px">
          <v-row align="center" style="margin-top: 60px">
            <span class="title">{{ nft.name }}</span>
            <v-spacer />
            <section v-if="itemPriceETH && !isOwned">
              <v-spacer></v-spacer>
              <v-icon color="red" right>mdi-ethereum</v-icon>
              <span class="body-1">{{ itemPriceETH }}</span>
              <v-btn
                width="150px"
                elevation="0"
                class="ml-5 redtext"
                @click="buyNow()"
                >buy</v-btn
              >
            </section>
          </v-row>
        </v-flex>

        <v-card-text>
          <v-list color="transparent">
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
          <v-list dense color="transparent">
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

        <v-card-actions v-if="isOwned">
          <v-spacer></v-spacer>
          <span class="title orange--text">this hyperpunk is sold</span>
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
      tokenID: null,
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
    this.init(this.id)
  },
  methods: {
    init(theID) {
      this.isOwned = false
      this.loadNFT(theID)
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
              title: 'Token ID: ' + theID,
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
              title: 'HyperPunks, augmented reality NFTs. 10,000 ',
            },
            {
              title:
                'programmatically generated characters, with their attributes,',
            },
            {
              title:
                'brought to the 3rd dimension and immortalized on the Ethereum blockchain,',
            },
            {
              title: 'Only 10000 hyperpunks will exist',
            },
          ],
          title: 'About Collection',
        },
      ]
    },
    loadNFT(id) {
      this.$axios
        .$get('https://hyp.s3.eu-west-2.amazonaws.com/json/' + id)
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
    searchForToken() {
      if (this.tokenID == null) {
        return
      }
      this.nft.animation_url = null
      this.id = this.tokenID
      this.init(this.tokenID)
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
// .v-list-item__subtitle {
//   font-size: 0.8em !important;
// }
</style>
