<template>
	<div class="">
		<!-- <no-ssr>
			<apex-chart
				type="rangeBar"
				height="300"
				:options="chartOptions"
				:series="series"
			/>
		</no-ssr> -->
		{{ sunrise }}
	</div>
</template>

<script>
export default {
	data() {
		return {
			times: null,
			sunrise: null,
			series: [
				{
					name: 'Sun',
					data: []
				}
			],
			chartOptions: {
				plotOptions: {
					bar: {
						horizontal: true
					}
				},
				yaxis: {
					min: new Date('2019-05-04').getTime(),
					max: new Date('2019-05-05').getTime()
				},
				xaxis: {
					type: 'datetime'
				},
				fill: {
					type: 'gradient',
					gradient: {
						shade: 'light',
						type: 'vertical',
						shadeIntensity: 0.25,
						gradientToColors: undefined,
						inverseColors: true,
						opacityFrom: 1,
						opacityTo: 1,
						stops: [50, 0, 100, 100]
					}
				}
			}
		}
	},
	asyncData(context) {
		// called every time before loading the component
		// as the name said, it can be async
		// Also, the returned object will be merged with your data object
		return {}
	},
	mounted() {
		this.fetchTimes()
	},
	methods: {
		async fetchTimes() {
			this.times = await this.$axios.$get(
				'https://api.sunrise-sunset.org/json?lat=27.427940&lng=-82.390970&date=today'
			)
			this.sunrise = Date.parse(this.times.results.sunrise).toString()
		}
	}
}
</script>
