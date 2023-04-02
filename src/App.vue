<template>
  <HeaderApp />
  <ExchangesForm :currList=currencyList @getCurrency="getCurrForExcahge" />
  <CurrencyList 
    @openModal="openModal" 
    :newCurrency="newCurrencyName"
    />
  <ModalApp 
    :isOpen="modalIsOpen"
    :mainCurrencyInList="mainCurrencyInList"
    @closeModal="closeModal"
    @addCurrency="addCurrency"
    mainText="Enter the currency abbreviation (for example: CHF, UAH)"
    title="Add currency to general sheet"
    />
</template>

<script>
import HeaderApp from './components/HeaderApp'
import ExchangesForm from './components/ExchangesForm'
import CurrencyList from './components/CurrencyList'
import ModalApp from './components/ModalApp'


export default {
  name: 'App',
  components: {
    HeaderApp,
    ExchangesForm,
    CurrencyList,
    ModalApp
  },
  data() {
    return {
      currencyList: {},
      startExchangeList: ['USD', 'EUR', 'UAH', 'GBP', 'BTC', 'ETH', 'BNB', 'XRP'],
      modalIsOpen: false,
      mainCurrencyInList:'USD',
      newCurrencyName:''
    }
  },
  methods: {
    getCurrForExcahge(data) {
      this.getCurrency(data, this.startExchangeList)
    },
    getCurrency(curr, currList) {
      fetch(`https://min-api.cryptocompare.com/data/price?fsym=${curr}&tsyms=${currList}`)
        .then(response => response.json())
        .then(data => {
          this.currencyList = data;
        });
    },
    openModal(data,curr) {
      this.modalIsOpen = data;
      this.mainCurrencyInList = curr;
    },
    closeModal(data){
      this.modalIsOpen = data;
    },
    addCurrency(data){
      this.newCurrencyName = data
    }

  },
  mounted() {
    this.getCurrency('USD', this.startExchangeList)
  }

}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

.container {
  padding: 20px;
}

body {
  margin: 0;
  background-color: #215C28;
}

.logo-container {
  min-height: 330px;
}

.sub-title {
  color: white;
  margin-top: 0;
}

h3 {
  font-size: 24px;
}
@media (min-width: 700px) {
  .container{
    width: 650px;
    margin: 0 auto;
    overflow: hidden;
  }
}
</style>
