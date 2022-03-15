<template>
    <div class="wrapper mt-6 p-6">
        <div v-if="isLoading" class="lds-spinner"><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div></div>
        <div v-else>
            <h2 class="header subtitle">Exchanges locales</h2>
            <p class="content">Acá vas a encontrar información, características, comisiones y links relevantes de los exchanges más populares que operan en Argentina.</p>
       
            <div class="exchange-card tile box" v-for="item in exchanges" :key="item.name">
                <div class="left-card-content">
                    <div class="card-tittle">
                        <a :href="item.url" target="_blank" >
                            <h2 class="card-header">{{item.name}}</h2>
                        </a>
                    </div>
                    <div>
                        <ul v-for="listItem in item.car" :key="listItem" class="is-flex">
                            <li>
                                - {{listItem}}
                            </li>
                        </ul>
                    </div>
                </div>
                <div class="middle-card-content ">
                    <div class="rates-display">
                        <h2 class="cotization-header">1 DAI / ${{Math.round(cclValue * item.rates.valorDai)}} ARS</h2>
                        <h6 v-if="item.rates.intAnual">Interes anual: {{item.rates.intAnual}} % </h6>
                        <h6 v-else>No da interes por holdear stables.</h6>
                    </div>
                    <div class="description">
                        {{item.desc}}
                    </div>
                </div>
                <div class="right-card-content">
                    <p>Fee compra: {{item.rates.feeCompra}} %</p>
                    <p>Fee venta: {{item.rates.feeVenta}} %</p>
                    <a :href="item.rates.url" target="_blank" > más información</a>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios';
export default {
    name: 'exchangeData',
    data: () => ({
        dollarData: [],
        cclValue: 0,
        exchanges: [
            {
                name: 'belo',
                desc: 'belo es un exchange reciente con una UI bastante simple y features innpvadoras. Te permite tener varias tarjetas virtuales, setear una orden de DCA y tiene bajas comisiones. Tambien tiene un esquema de cashback que va desde el 0.2% a 20%.',
                car: ['tarjeta visa', 'inversiones', 'cashback random', 'soporte para bsc', 'fondear globalmente'],
                url:'https://www.belo.app/',
                eaIndex: 0.7,
                rates: {
                    valorDai: (1 + 0.018),
                    intAnual: 7,
                    feeCompra: 1,
                    feeVenta: 0.5,
                    url: 'https://help.belo.app/es/articles/5215371-minimos-y-comisiones-de-las-transferencias-cripto'
                }
            },
            {
                name: 'Lemon Cash',
                desc: 'Lemon Cash es un exchange nuevo con mmuchas features innovadoras como cashback, tarjeta Visa, promociones y mucha inversión en marketing. En cuanto a UI no es de los mejores pero cumple con las opciones basicas.',
                car: ['tarjeta visa', 'inversiones', 'cashback en btc', 'soporte para bsc','fondear globalmente'],
                url:'https://www.lemon.me/',
                eaIndex: 0.5,
                rates: {
                    valorDai: (1 + 0.02),
                    intAnual: 13,
                    feeCompra: 1,
                    feeVenta: 0.5,
                    url: 'https://help.lemon.me/es/articles/5017589-comisiones-y-fees'
                }
            },
            {
                name: 'Buenbit',
                desc: 'Buenbit es un exchange "legacy" de argentina, se supo mantener al día con su interfaz y features pero no tiene los mejores rendimientos del mercado.',
                car: ['tarjeta visa', 'inversiones', 'cashback', 'soporte para bsc','fondear globalmente','rendimientos en todas las cryptos soportadas'],
                eaIndex: 0.2,
                url:'https://www.buenbit.com/es-ar',
                rates: {
                    valorDai: (1 + 0.018),
                    intAnual: 10.25,
                    feeCompra: 0,
                    feeVenta: 0,
                    url: 'https://ayuda.buenbit.com/es/articles/3535089-cobran-comision-por-la-compra-y-venta#:~:text=No%20cobramos%20ninguna%20comisi%C3%B3n%20por,los%20diferentes%20mercados%20que%20ofrecemos.'
                }
            },
            {
                name: 'ripio',
                desc: 'Ripio es un exchange legacy que ya quedó un poco outdated en cuanto a features. No tiene tarjeta y tiene comisiones altas respecto a la competencia. Tiene a favor que es una plataforma con trayectoria y bastante conocida.',
                car: ['inversiones', 'Ripio coin'],
                url:'https://www.ripio.com/ar/',
                eaIndex: 0.2,
                rates: {
                    valorDai: (1 + 0.042),
                    intAnual: null,
                    feeCompra: 1,
                    feeVenta: 0.5,
                    feeFund: 3,
                    url:'https://help.ripio.com/hc/es/articles/360025033253-Comisiones-por-operaciones-en-Ripio'
                }
            }
        ]
    }),
    mounted () {
        this.getCclData()
     },
    computed: {
        isLoading(){
            if(this.cclValue == 0){
                return true
            }
            return false
        },
    },
    methods: {
        async getCclData(){
            //this.isLoading = true;
            let response = await axios.get('https://www.dolarsi.com/api/api.php?type=valoresprincipales')
            const data = Array.from(response.data)
            this.cclPrice(data)
            //this.isLoading = false;
        },
        cclPrice(payload){
            payload.forEach(item => {
                if(item.casa.nombre === 'Dolar Contado con Liqui'){
                    this.cclValue = parseInt (item.casa.compra)
                }
            })
        }
        
    }
}
</script>
<style scoped lang="scss">

.wrapper{
    background: var(--secondaryBackground);
    min-height: 1000px;
    .header{
        color: var(--mainTextColor);
        font-size: 20px;
    }
    .content{
         color: var(--secondaryTextColor);
        font-size: 18px;
    }
    
    .exchange-card{
        background: var(--mainBackground);
        color: var(--secondaryTextColor);
        position: relative;
        display: flex;
        justify-content: space-between;
        .card-header{
            color: var(--mainTextColor);
            font-size: 20px;
            font-weight: 600;
        }
        .ea-index{
            color: var(--secondaryTextColor);
            padding-left: 0 !important;
        }
        .cotization-header{
            font-size: 22px;
            font-weight: 600;
        }
         .left-card-content{
            width: 200px;
        }
        .middle-card-content{
            width: 900px;
            // margin-left: auto;
        }
        .right-card-content{
            width: 180px;
            display: flex;
            flex-direction: column;
            // position: absolute;
            // right: 0;
            padding-right: 30px;
        }
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
    .search-bar{
        width: 600px;
        margin: auto;
        >input{ 
            background: var(--mainBackground);
            border: none;
            &:hover{
                transition: 0.1s;
                background: rgba(255, 255, 255, 0.103);
            }
        }
    }
    .table-container{
        width: 100%;
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