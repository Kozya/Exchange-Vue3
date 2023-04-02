<script >
export default {
    name: 'ExchangesForm',
    props: {
        newCurrency: String,
    },
    data() {
        return {
            mainCurrency: 'USD',
            currencyList: {},
            listCurrencyOption: [
                { value: 'USD' },
                { value: 'EUR' },
                { value: 'UAH' }
            ],
            currencyForReq: ['USD', 'EUR', 'UAH', 'BTC', 'ETH'],
            canRefresh: true,
        }
    },
    methods: {
        getListCurrency(curr, currList) {
            fetch(`https://min-api.cryptocompare.com/data/price?fsym=${curr}&tsyms=${currList}`)
                .then(response => response.json())
                .then(data => {
                    this.currencyList = data;
                });
        },
        getMainCurrency() {
            this.getListCurrency(this.mainCurrency, this.currencyForReq)
        },
        refreshCurrency() {
            if (this.canRefresh) {
                this.getListCurrency(this.mainCurrency, this.currencyForReq)
                this.canRefresh = false;

                setTimeout(() => {
                    this.canRefresh = true;
                }, 5000);
            }
        },
        openModal() {
            this.$emit('openModal', true, this.mainCurrency)

        }

    },
    mounted() {
        let localList = JSON.parse(localStorage.getItem("currencyForReq"));
        if (localList) {
            this.currencyForReq = localList;
        }
        this.getListCurrency(this.mainCurrency, this.currencyForReq)
    },
    watch: {
        newCurrency: function (value) {
            if (value) {
                this.currencyForReq.push(value);
                localStorage.setItem('currencyForReq', JSON.stringify(this.currencyForReq));
                this.getListCurrency(this.mainCurrency, this.currencyForReq);
            }
        }
    },
}


</script>

<template>
    <div class="container">
        <h3 class="sub-title">list of exchange rates</h3>
        <div class="exchange-rates-wrapper">
            <div class="rates-top-block">
                <select v-model="mainCurrency" @change="getMainCurrency">
                    <option v-for="option in listCurrencyOption" :value="option.value" :key="option.value">
                        {{ option.value }}
                    </option>
                </select>
                <div v-if="canRefresh" @click="refreshCurrency" class="refrech-btn">Refresh cyrrency
                </div>
            </div>

            <div class="currency-list">
                <div v-for="(value, item) of currencyList" :key="item">
                    <div class="list-item" v-if="item !== mainCurrency">{{ item }} - {{ value }}</div>
                </div>
            </div>
            <button class="open-modal-btn" @click="openModal">add cyrrency</button>
        </div>

    </div>
</template>



<style scoped>
.exchange-rates-wrapper {
    background-color: #48484A;
    border-radius: 4px;
    padding: 20px;
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    justify-content: space-between;
    margin-bottom: 30px;
}

.list-item {
    color: white;
    text-align: left;
    margin-bottom: 5px;
}

.exchange-rates-wrapper select {
    width: 30%;
    height: 30px;
    border-radius: 4px;
    padding-left: 5px;
}

.rates-top-block {
    width: 100%;
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin-bottom: 15px;
}

.refrech-btn {
    color: white;
    cursor: pointer;
    text-decoration: underline;
}

.open-modal-btn {
    width: 50%;
    height: 30px;
    border: 1px solid black;
    display: flex;
    align-items: center;
    justify-content: center;
    background: white;
    border-radius: 4px;
    margin-top: 15px;
    cursor: pointer;
}
</style>
