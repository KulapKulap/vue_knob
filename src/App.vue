<template>
  <div class="wrapper">
    <!-- <h1>Crypto<b style="opacity:1 !important">X</b>change</h1>  -->
  
    <video autoplay muted>
      <source src="/Users/jinhua/Desktop/PROJECTS_20240521/vue_bitcoin/vue_bitcoin/src/assets/videos/preview.mp4" type="video/quicktime"> 
    </video>
    <div class="wrapper-inner">
      <div class="wrapper-inner-input">
        <Input :changeAmount="changeAmount" :convert="convert" :favorite="favorite" :error="error"/>
        <Favorite :favs="favs" v-if="favs.length > 0" :getFromFavs="getFromFavs"/>
      </div>
      <p v-if="error != ''">{{ error }}</p>
      <p v-if="result != 0">{{ result }}</p>
      <div class="selectors">
        <Selector :setCrypto="setCryptoFirst" :cryptoNow="cryptoFirst"/>
        <Selector :setCrypto="setCryptoSecond" :cryptoNow="cryptoSecond"/>
      </div>
    </div>
  </div>
</template>

<script>
import Input from './components/Input.vue'
import Selector from './components/Selector.vue'
import Favorite from './components/Favorite.vue'
import CryptoConvert from 'crypto-convert';

const convert = new CryptoConvert();

export default {
  components: { Input, Selector, Favorite },
  data() {
    return {
      amount: 0,
      cryptoFirst: '',
      cryptoSecond: '',
      error: '',
      result: 0,
      favs: []
    };
  },
  methods: {
    changeAmount(val) {
      this.amount = val;
    },
    setCryptoFirst(val) {
      this.cryptoFirst = val;
    },
    setCryptoSecond(val) {
      this.cryptoSecond = val;
    },
    async convert() {
      if (this.amount <= 0) {
        this.error = 'Enter amount > 0';
        return;
      } else if (this.cryptoFirst == this.cryptoSecond) {
        this.error = 'First and second currency must differ';
        return;
      } else if (this.cryptoFirst == '' || this.cryptoSecond == '') {
        this.error = 'Choose a currency';
        return;
      }
      this.error = '';

      await convert.ready();

      if (this.cryptoFirst == 'BTC' && this.cryptoSecond == 'ETH')
        this.result = convert.BTC.ETH(Math.floor(this.amount));
      else if (this.cryptoFirst == 'BTC' && this.cryptoSecond == 'USD')
        this.result = convert.BTC.USD(Math.floor(this.amount));
      else if (this.cryptoFirst == 'ETH' && this.cryptoSecond == 'USD')
        this.result = convert.ETH.USD(Math.floor(this.amount));
      else if (this.cryptoFirst == 'ETH' && this.cryptoSecond == 'BTC')
        this.result = convert.ETH.BTC(Math.floor(this.amount));
      else if (this.cryptoFirst == 'USD' && this.cryptoSecond == 'BTC')
        this.result = convert.USD.BTC(Math.floor(this.amount));
      else if (this.cryptoFirst == 'USD' && this.cryptoSecond == 'ETH')
        this.result = convert.USD.ETH(Math.floor(this.amount));
    },
    favorite() {
      this.favs = [{
        'from': this.cryptoFirst,
        'to': this.cryptoSecond
      }];
    },
    getFromFavs(index) {
      this.cryptoFirst = this.favs[index].from,
      this.cryptoSecond = this.favs[index].to
    }
  }
};
</script>

<style scoped>
/**/
  .wrapper {
    height: 100vh;
    width: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
  }

  .wrapper-inner {
   /* border: 2px solid rgb(83, 24, 187);*/
    width: 20%;
  }

  .wrapper-inner-input {
    display: flex;
    flex-direction: row;
    justify-content: space-around;
    margin: 5px 5px;
  }

  .wrapper-inner-input input,
  .wrapper-inner-input button {
    width: 30%;
  }

  .selectors {
    display: flex;
    justify-content: space-around;
    margin: 0 auto;
  }

  h1 {
    text-align: center;
    margin-top: 60px;
    font-size: 10em;
    opacity: .3;
  }

  h1 b {
    opacity: 1 !important;
    color: rgb(255, 0, 0);
  }

  video{
    /*border: 2px solid rgb(83, 24, 187);*/
  }




  @media screen and (min-width: 992px) {
    .wrapper-inner {
      width: 30%;
    }

    .selectors {
      justify-content: space-between;
    }

    h1 {
      font-size: 10em;
    }

    video{
      width:50%;
    }
  }

  @media screen and (min-width: 768px) and (max-width: 991px) {
    .wrapper-inner {
      width: 70%;
    }

    .selectors {
      justify-content: space-around;
    }

    h1 {
      font-size: 5em;
    }

    video{
      width:90%;
    }
  }

  @media screen and (max-width: 767px) {
    .wrapper-inner {
      width: 50%;
    }

    h1 {
      font-size: 2em;
    }

    video{
      width:90%;
    }
  }
</style>
