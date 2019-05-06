<template>
	<div class="">
		<h1>Humidity</h1>
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
export default {
	data() {
		return {
			series: [
				{
					name: 'Humidity %',
					type: 'area',
					data: []
				}
			],
			chartOptions: {
				grid: {
					row: {
						colors: ['#f3f3f3', 'transparent'], // takes an array which will be repeated on columns
						opacity: 0.5
					}
				},
				toolbar: {
					autoSelected: 'zoom'
				},
				colors: ['#6574CD'],
				zoom: {
					type: 'x',
					enabled: true
				},
				stroke: {
					curve: 'smooth'
				},
				fill: {
					type: 'solid',
					opacity: [0.2, 1]
				},
				xaxis: {
					type: 'datetime',
					labels: {
						datetimeFormatter: {
							year: 'yyyy',
							month: "MMM 'yy",
							day: 'dd MMM',
							hour: 'HH:mm:ss'
						}
					}
				},
				yaxis: {
					max: 100,
					min: 0,
					title: {
						text: 'Humidity %'
					}
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
				'http://192.168.1.14:8000/api/humidity'
			)
			this.series[0].data = this.initalData.map(function(data) {
				return { x: Date.parse(data.measurement_time), y: data.percentage }
			})
		},
		updateChart(data) {
			this.series[0].data.push({
				x: Date.parse(data.humidity.measurement_time),
				y: data.humidity.percentage
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
