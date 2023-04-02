<script>
export default {
    name: "ExchangesForm",
    props: {
        currList: Object,
    },
    data() {
        return {
            mainCurrency: "USD",
            secondaryCurrency: "UAH",
            mainCurrencyValue: "",
            secondaryCurrencyValue: "",
        };
    },
    methods: {
        getMainCurrency(mainCurrency) {
            this.$emit("getCurrency", mainCurrency);
            this.mainCurrencyValue = this.secondaryCurrencyValue = "";
        },
        getResultOfExchange(event, currency) {
            if (currency === "mainCurrency") {
                this.mainCurrencyValue = event.target.value;
                this.secondaryCurrencyValue = event.target.value * this.currList[this.secondaryCurrency];
            } else {
                this.secondaryCurrencyValue = event.target.value;
                this.mainCurrencyValue = event.target.value / this.currList[this.secondaryCurrency];
            }
        },
        getSelectedResult(value) {
            this.secondaryCurrencyValue = value * this.currList[this.secondaryCurrency];
        }
    },
    mounted() { },
};
</script>

<template>
    <div class="container">
        <div class="logo-container">
            <img src="../assets/img/welcome-image.png" alt="logo" />
        </div>
        <h2 class="sub-title">currency exchange</h2>
        <form class="exchange-form" action="">
            <div class="active-currency">{{ mainCurrency }}</div>
            <div class="main-curr-wrapper">
                <select v-model="mainCurrency" @change="getMainCurrency(mainCurrency)">
                    <option v-for="(key, currName) of currList" :value="currName" :key="currName">
                        {{ currName }}
                    </option>
                </select>
                <input @input="getResultOfExchange($event, 'mainCurrency')" :value="mainCurrencyValue"
                    placeholder="Enter value" type="number" />
            </div>
            <div class="active-currency">{{ secondaryCurrency }}</div>
            <div class="secondary-curr-wrapper">
                <select v-model="secondaryCurrency" @change="getSelectedResult(this.mainCurrencyValue)">
                    <option disabled :value="secondaryCurrency">
                        {{ secondaryCurrency }}
                    </option>
                    <option v-for="(key, currName) of currList" :value="currName" :key="currName">
                        {{ currName }}
                    </option>
                </select>
                <input @input="getResultOfExchange($event, 'secondaryCurrency')" :value="secondaryCurrencyValue"
                    placeholder="Enter value" type="number" />
            </div>
        </form>
    </div>
</template>

<style scoped>
.logo-container img {
    width: 100%;
    height: 100%;
}

.exchange-form {
    background: linear-gradient(74.34deg, #5bd09a 8.45%, #00da1c 78.04%);
    border-radius: 4px;
    padding: 20px;
}

.logo-container {
    width: 100%;
}

.main-curr-wrapper,
.secondary-curr-wrapper {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
}

.main-curr-wrapper select,
.secondary-curr-wrapper select {
    width: 30%;
    height: 30px;
    border-radius: 4px;
    margin-bottom: 15px;
    padding-left: 5px;
}

.main-curr-wrapper input,
.secondary-curr-wrapper input {
    width: 50%;
    height: 30px;
    border: 1px solid black;
    border-radius: 4px;
    margin-bottom: 15px;
    padding-left: 7px;
}
</style>
