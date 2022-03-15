<template>
    <div class="wrapper mt-6 p-6">
        <div v-if="isLoading" class="lds-spinner"><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div></div>
        <div v-else>
            <h1 class="header mb-4">
                    Cotizaciones
            </h1>
            <div class="ratesWrapper tile is-ancestor mt-2 mb-6">
                <div v-for="item of dolarData" :key="item.casa.nombre">
                    <div class="ratesContainer tile box mx-2 is-size-7" v-if="item.casa.venta != 0 && item.casa.variacion">
                        <p>{{item.casa.nombre}}</p>
                        <div class="top-row is-flex ">
                            <p class="px-2">Compra: {{item.casa.compra}}</p>
                            <p>Venta: {{item.casa.venta}}</p>
                        </div>
                        <div class="is-flex bottom-row">
                            <p v-if="item.casa.variacion" :class="Math.sign(parseInt(item.casa.variacion)) == 1 ? 'upwards' : 'downwards' ">{{item.casa.variacion}} %</p>
                        </div>
                    </div>
                </div>
            </div>
            <div class="table-container px-6 pt-4">
                <h1 class="header my-2">
                    Lista de Coins
                </h1>
                <input class="input" type="text" v-model="search" placeholder="Filtrar por nombre">
                <table class="table is-fullwidth is-hoverable " >
                    <thead class="coin-table-head">
                        <tr>
                            <th>TICKET</th>
                            <th>MONEDA</th>
                            <th>VARIACIÓN ult 24hs</th>
                            <th>PRECIO</th>
                            <th>ATH %</th>
                            <th>MARKET CAP</th>
                        </tr>
                    </thead>
                    <tbody class="coin-table-body" v-for="row of getFilteredCoinList" :key="row">
                        <tr>
                            <td>{{row.symbol.toUpperCase()}}</td>
                            <td>{{row.name}}</td>
                            <td :class="Math.sign(row.price_change_percentage_24h) === 1 ? 'upwards' : 'downwards' ">{{row.price_change_percentage_24h}} %</td>
                            <td>U$ {{row.current_price}}</td>
                            <td :class="Math.sign(row.ath_change_percentage) === 1 ? 'upwards' : 'downwards' ">{{row.ath_change_percentage}} %</td>
                            <td>{{row.market_cap}} / {{row.market_cap_change_percentage_24h}} %</td>
                        </tr>
                    </tbody>
                </table>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios';
export default {
    name: 'dashboard',
    data: () => ({
        dataList :null,
        dolarData: null,
        search: null
    }),
    mounted () {
        this.getCoinList(),
        this.getDolarRates()
     },
    computed: {
         getFilteredCoinList() {
			if (!this.dataList) return [];
			if (!this.search) return this.dataList;

			const regex = new RegExp(this.search, 'gi');
			return this.dataList.filter(t => t.name.match(regex)).sort((a, b) => `${a.name}`.localeCompare(b.name));
		},
        isLoading(){
            if( this.dataList == null || this.dolarData == null){
                return true
            }
            return false
        }

    },
    methods: {
        async getCoinList(){
            this.isLoading = true
            await axios .get('https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=100&page=1&sparkline=false')
                .then(response => (this.dataList = response.data, console.log('crypto',response)));
            this.isLoading = false
        },
        async getDolarRates(){
            await axios.get('https://www.dolarsi.com/api/api.php?type=valoresprincipales')
                .then(response => (this.dolarData = response.data, console.log('usd',response)))
 
        },
    }
}
</script>
<style scoped lang="scss">
$input-placeholder-color: var(--secondaryTextColor);
@import "bulma";
.wrapper{
    background: var(--secondaryBackground);
    min-height: 1000px;
     .header{
            color: var(--mainTextColor);
            font-size: 20px;
        }
    .ratesWrapper{
        justify-content: center;
        .ratesContainer{
            flex-direction: column;
            padding: 10px 5px !important;
            background: var(--mainBackground);
            color: var(--secondaryTextColor);
            .bottom-row{
                justify-content: center;
            }
            .upwards{
                color: var(--success);
            }
            .downwards{
                color: var(--error);
            }
        }
    }
    .table-container{
        width: 100%;
        overflow: hidden;
        .input{
            background: var(--mainBackground);
            border: none;
            margin-bottom: 20px;
            &:hover{
                transition: 0.1s;
                background: #212633;
            }
        }
        .table{
            color: var(--mainTextColor);
            background: var(--secondaryBackground);
            > thead > tr > th{
                color: var(--mainTextColor);
            }
            .coin-table-head{
                >tr{
                color: var(--mainTextColor);
                }
            }
            .coin-table-body >tr{
                &:hover{
                    background: rgba(255, 255, 255, 0.027);
                }
            }

            .upwards{
                color: var(--success);
            }
            .downwards{
                color: var(--error);
            }
        }

    }
    .lds-spinner {
    color: official;
    display: inline-block;
    position: relative;
    width: 80px;
    height: 80px;
    }
    .lds-spinner div {
    transform-origin: 40px 40px;
    animation: lds-spinner 1.2s linear infinite;
    }
    .lds-spinner div:after {
    content: " ";
    display: block;
    position: absolute;
    top: 3px;
    left: 37px;
    width: 6px;
    height: 18px;
    border-radius: 20%;
    background: #fff;
    }
    .lds-spinner div:nth-child(1) {
    transform: rotate(0deg);
    animation-delay: -1.1s;
    }
    .lds-spinner div:nth-child(2) {
    transform: rotate(30deg);
    animation-delay: -1s;
    }
    .lds-spinner div:nth-child(3) {
    transform: rotate(60deg);
    animation-delay: -0.9s;
    }
    .lds-spinner div:nth-child(4) {
    transform: rotate(90deg);
    animation-delay: -0.8s;
    }
    .lds-spinner div:nth-child(5) {
    transform: rotate(120deg);
    animation-delay: -0.7s;
    }
    .lds-spinner div:nth-child(6) {
    transform: rotate(150deg);
    animation-delay: -0.6s;
    }
    .lds-spinner div:nth-child(7) {
    transform: rotate(180deg);
    animation-delay: -0.5s;
    }
    .lds-spinner div:nth-child(8) {
    transform: rotate(210deg);
    animation-delay: -0.4s;
    }
    .lds-spinner div:nth-child(9) {
    transform: rotate(240deg);
    animation-delay: -0.3s;
    }
    .lds-spinner div:nth-child(10) {
    transform: rotate(270deg);
    animation-delay: -0.2s;
    }
    .lds-spinner div:nth-child(11) {
    transform: rotate(300deg);
    animation-delay: -0.1s;
    }
    .lds-spinner div:nth-child(12) {
    transform: rotate(330deg);
    animation-delay: 0s;
    }
    @keyframes lds-spinner {
    0% {
        opacity: 1;
    }
    100% {
        opacity: 0;
    }
    }
}
</style>