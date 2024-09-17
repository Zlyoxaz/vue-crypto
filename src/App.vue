
<template>
	<h1>CRYPTO</h1>
	<Input :changeAmount="changeAmount" :convert="convert" :favourite="favourite" />
	<p v-if="error != ''" class="error">{{ error }}</p>
	<p v-if="result != 0" class="result-text">{{ result }}</p>
	<Favourite :uniqFavs="uniqFavs" v-if="uniqFavs.length > 0" :getFromFavs="getFromFavs" />

	<div class="selectors">
		<Selector :setCrypto="setCryptoFirst"  :cryptoNow="cryptoFirst"/>
		<Selector :setCrypto="setCryptoSecond" :cryptoNow="cryptoSecond"/>
	</div>
</template>


<script>
import Input from './components/Input.vue'
import Selector from './components/Selector.vue'
import Favourite from './components/Favourite.vue'
import CryptoConvert from 'crypto-convert'

const convert = new CryptoConvert();


export default {
	props: {

	},
	components: { Input, Selector, Favourite },
  	data() {
		return {
			amount: 0,
			cryptoFirst: '',
			cryptoSecond: '',
			error: '',
			result: 0,
			uniqFavs: []
		}
	},
	methods: {
		favourite() {
			if(this.cryptoFirst == '' && this.cryptoSecond == '') 
				return this.error = "Не выбрана ни одна пара"

			else if (this.cryptoFirst == this.cryptoSecond) {
				this.error = "Выбраны одинаковые валютные пары"
				return;
			}

			const search = this.uniqFavs.find((el) => {
				if (el.from === this.cryptoFirst && el.to === this.cryptoSecond) {
					this.error = "Выбранная пара уже в избранном"
					return el;
				}
			})

			if (!search) {
				this.uniqFavs.push({
					from: this.cryptoFirst,
					to: this.cryptoSecond
				});
			}
		},
		getFromFavs(index) {
			this.cryptoFirst = this.uniqFavs[index].from
			this.cryptoSecond = this.uniqFavs[index].to

		},
		changeAmount(val) {
			this.amount = val
		},
		setCryptoFirst(val) {
			this.cryptoFirst = val
		},
		setCryptoSecond(val) {
			this.cryptoSecond = val
		},
		async convert() {

			if (this.amount <= 0) {
				this.error = 'Введите число больше чем 0'
				return;
			} else if (this.cryptoFirst == this.cryptoSecond) {
				this.error = "Выбраны одинаковые валютные пары"
				return;
			} else if (this.cryptoFirst == '' || this.cryptoSecond == '') {
				this.error = 'Выберите валюту'
				return;
			}

			this.error = '';

			await convert.ready()

			if(this.cryptoFirst == 'BTC' && this.cryptoSecond == 'ETH')
				this.result = convert.BTC.ETH(this.amount);
			else if(this.cryptoFirst == 'BTC' && this.cryptoSecond == 'USDT')
				this.result = convert.BTC.USDT(this.amount);
			else if(this.cryptoFirst == 'ETH' && this.cryptoSecond == 'BTC')
				this.result = convert.ETH.BTC(this.amount);
			else if(this.cryptoFirst == 'ETH' && this.cryptoSecond == 'USDT')
				this.result = convert.ETH.USDT(this.amount);
			else if(this.cryptoFirst == 'USDT' && this.cryptoSecond == 'BTC')
				this.result = convert.USDT.BTC(this.amount);
			else if(this.cryptoFirst == 'USDT' && this.cryptoSecond == 'ETH')
				this.result = convert.USDT.ETH(this.amount);
		}
	}
}
</script>


<style scoped>
.selectors {
	display: flex;
	justify-content: space-around;
	width: 700px;
	margin: 0 auto;
}

.error {
    color: red;
}

.result-text {
	font-family: 'Nabla', cursive;
	font-size: 2em;
}

</style>