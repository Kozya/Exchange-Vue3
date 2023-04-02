<script >
export default {
    name: 'ModalApp',
    props: {
        title: String,
        mainText: String,
        isOpen: {
            type: Boolean,
            default: false,
        },
        mainCurrencyInList: String,
    },
    data() {
        return {
            currencyList: {},
            searchCurrencyValue: '',
            newCurrency: '',
            isOpenModal: false,
            isInfo: '',
            classError: false,
            btnIsActivate: false,
        };
    },
    methods: {
        searchNewCurrency() {
            fetch(`https://min-api.cryptocompare.com/data/price?fsym=${this.mainCurrencyInList}&tsyms=${this.searchCurrencyValue}`)
                .then(response => response.json())
                .then(data => {
                    if (data.Response === 'Error') {
                        this.isInfo = data.Message;
                        this.classError = true;
                    } else {
                        this.btnIsActivate = true;
                        this.isInfo = `${Object.keys(data)} to ${this.mainCurrencyInList} - ${Object.values(data)}`;
                        this.newCurrency = `${Object.keys(data)}`;
                        this.classError = false;
                    }

                });
        },
        addCurrencyToGeneralList() {
            this.$emit('addCurrency', this.newCurrency);
            this.closeModal();
        },
        closeModal() {
            this.isOpenModal = false;
            this.$emit('closeModal', this.isOpenModal);
            this.classError = false;
            this.isInfo = this.searchCurrencyValue = '';
        }
    },
    watch: {
        isOpen: function (value) {
            if (value) {
                this.btnIsActivate = false;
                this.isOpenModal = value;
            }
        }
    },

}


</script>

<template>
    <div class="modal-wrapper" v-if="isOpenModal">
        <div @click="closeModal" class="bg-modal"></div>
        <div class="modal-body">
            <div @click="closeModal" class="modal-close-btn">x</div>
            <p class="title">{{ title }}</p>
            <p class="main-text">{{ mainText }}</p>
            <div class="search-btn-wrapper">
                <input placeholder="Enter currency" maxlength="3" v-model="searchCurrencyValue" type="text">
                <button @click="searchNewCurrency">Search</button>
            </div>
            <div v-if="isInfo" :class="[{ 'error': classError }, 'info-block']">{{ isInfo }}</div>
            <div class="modal-footer-btn">
                <button @click="addCurrencyToGeneralList" :class="[{ 'disabled-btn': !btnIsActivate }, 'add-btn']">Add to
                    general list</button>
            </div>
        </div>

    </div>
</template>



<style scoped>
.title {
    font-size: 24px;
    margin-top: 0;
}

.add-btn.disabled-btn {
    pointer-events: none;
    cursor: not-allowed;
    color: #48484A;
    background-color: #0f0f0f;
}

.add-btn {
    margin-top: 20px;
    width: 50%;
    height: 30px;
    background-color: #215C28;
    color: white;
    border: 1px solid black;
    border-radius: 4px;
}

.info-block {
    position: absolute;
    font-size: 12px;
    color: #5bd09a;
}

.info-block.error {
    color: red;
}

.search-btn-wrapper {
    width: 100%;
    display: flex;
    align-items: center;
    justify-content: space-between;
}

.search-btn-wrapper input {
    width: 50%;
    height: 26px;
    border: 1px solid black;
    border-radius: 4px;
    margin-bottom: 15px;
    padding-left: 7px;
}

.search-btn-wrapper button {
    width: 30%;
    height: 30px;
    border: 1px solid black;
    border-radius: 4px;
    margin-bottom: 15px;
    padding-left: 7px;
    background-color: #48484A;
    color: white;
}

.modal-body {
    width: 80%;
    min-height: 200px;
    background-color: white;
    padding: 10px;
}

.modal-wrapper {
    position: absolute;
    top: 0;
    width: 100%;
    height: 100%;
}

.bg-modal {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, .5);
    z-index: 20;
    transition: .3s all;
}

.modal-body {
    width: 80%;
    max-width: 500px;
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    z-index: 30;
    box-shadow: 0 3px 10px -.5px rgba(0, 0, 0, .2);
    text-align: center;
    padding: 30px;
    border-radius: 3px;
    background-color: #fff;
    transition: 0.3s all;
}

.modal-close-btn {
    width: 15px;
    height: 15px;
    position: absolute;
    top: 10px;
    right: 10px;
    cursor: pointer;
}
</style>
