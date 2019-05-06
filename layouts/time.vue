<template>
	<div class="pt-8">
		<div class="text-grey text-2xl text-center">
			{{ currentTime }}
		</div>
		<div class="flex text-center pt-20">
			<div class="flex-1 text-grey text-5xl">
				<snurise />
				<div class="text-xl">{{ snuriseTime | toEast }}</div>
			</div>
			<div class="flex-1 text-grey text-5xl">
				<sunset />
				<div class="text-xl">{{ sunsetTime | toEast }}</div>
			</div>
		</div>
	</div>
</template>

<script>
import sunset from 'vue-material-design-icons/WeatherSunsetDown.vue'
import snurise from 'vue-material-design-icons/WeatherSunsetUp.vue'
import moment from 'moment'
export default {
	components: {
		sunset,
		snurise
	},
	filters: {
		toEast: function(value) {
			return moment(value).format('h:mm:ss a')
		}
	},
	data() {
		return {
			currentTime: new Date(),
			response: null,
			snuriseTime: null,
			sunsetTime: null
		}
	},
	asyncData(context) {
		// called every time before loading the component
		// as the name said, it can be async
		// Also, the returned object will be merged with your data object
		return {}
	},
	created() {
		setInterval(
			() => (this.currentTime = moment().format('dddd, MMMM Do YYYY, h:mm:ss a')),
			1000
		)
	},
	mounted() {
		this.fetchData()
	},
	methods: {
		moment: () => {
			return moment()
		},
		async fetchData() {
			this.response = await this.$axios.$get(
				'https://api.sunrise-sunset.org/json?lat=27.427940&lng=-82.390970&date=today&formatted=0'
			)
			this.snuriseTime = this.response.results.sunrise
			this.sunsetTime = this.response.results.sunset
		}
	}
}
</script>
