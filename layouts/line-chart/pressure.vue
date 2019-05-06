<template>
	<div class="">
		<div class="text-grey text-xl text-center">
			<span class="pr-2"><wind /></span>Air Pressure
		</div>
		<no-ssr>
			<apex-chart
				ref="realtimeChart"
				type="line"
				height="500"
				:options="chartOptions"
				:series="series"
			/>
		</no-ssr>
	</div>
</template>

<script>
import Echo from 'laravel-echo'
import wind from 'vue-material-design-icons/AirFilter.vue'
export default {
	components: {
		wind
	},
	data() {
		return {
			series: [
				{
					name: 'mb',
					type: 'area',
					data: []
				}
			],
			chartOptions: {
				toolbar: {
					autoSelected: 'zoom'
				},
				colors: ['#4DC0B5'],
				zoom: {
					type: 'x',
					enabled: true
				},
				fill: {
					type: 'gradient',
					gradient: {
						type: 'vertical',
						opacityFrom: 0.8,
						opacityTo: 0.01,
						stops: [0, 100],
						colorStops: []
					}
				},
				stroke: {
					show: true,
					curve: 'smooth',
					width: 2
				},
				grid: {
					yaxis: {
						lines: {
							show: false
						}
					}
				},
				xaxis: {
					type: 'datetime',
					labels: {
						datetimeFormatter: {
							year: 'yyyy',
							month: "MMM 'yy",
							day: 'dd MMM',
							hour: 'HH:mm:ss'
						},
						style: {
							colors: 'gray'
						}
					}
				},
				yaxis: {
					labels: {
						style: {
							color: 'gray'
						}
					},
					forceNiceScale: true
				}
			},
			initalData: null
		}
	},
	asyncData(context) {
		// called every time before loading the component
		// as the name said, it can be async
		// Also, the returned object will be merged with your data object
		return {}
	},
	mounted() {
		this.fetchData()
		this.connect()
	},
	methods: {
		async fetchData() {
			this.initalData = await this.$axios.$get(
				'http://192.168.1.14:8000/api/pressure'
			)
			this.series[0].data = this.initalData.map(function(data) {
				return { x: Date.parse(data.measurement_time), y: data.millibars }
			})
		},
		updateChart(data) {
			this.series[0].data.push({
				x: Date.parse(data.pressure.measurement_time),
				y: data.pressure.millibars
			})
		},
		connect() {
			if (!this.echo) {
				this.echo = new Echo({
					broadcaster: 'pusher',
					key: 'OoE1bixAvTPlI75uNqR5',
					cluster: 'mt1',
					encrypted: false,
					wsHost: '127.0.0.1',
					wsPort: '6001'
				})
				this.echo.channel('weather').listen('WeatherMeasured', message => {
					this.updateChart(message)
				})
			}
		}
	}
}
</script>
