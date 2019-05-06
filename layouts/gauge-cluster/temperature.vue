<template>
	<div class="p-4">
		<no-ssr>
			<apex-chart
				type="radialBar"
				height="300"
				:options="chartOptions"
				:series="[roundedTemp]"
			/>
		</no-ssr>
	</div>
</template>

<script>
import Echo from 'laravel-echo'
export default {
	data() {
		return {
			temperature: 0,
			chartOptions: {
				fill: {
					colors: ['#3490DC'],
					type: 'gradient',
					gradient: {
						type: 'horizontal',
						gradientToColors: ['#E3342F'],
						stops: [10, 90]
					}
				},
				plotOptions: {
					radialBar: {
						startAngle: -135,
						endAngle: 135,
						dataLabels: {
							name: {
								show: true,
								fontSize: '18px',
								color: '#B8C2CC'
							},
							value: {
								show: true,
								color: '#B8C2CC',
								fontSize: '18px',
								formatter: function(val, opts) {
									return val + ' °F'
								}
							}
						},
						track: {
							opacity: 0.2
						}
					}
				},
				labels: ['Temperature']
			}
		}
	},
	computed: {
		roundedTemp: function() {
			return parseFloat(this.temperature).toFixed(2)
		}
	},
	mounted() {
		this.connect()
	},
	methods: {
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
					this.updateData(message)
				})
			}
		},
		updateData(data) {
			this.temperature = data.temperature.degrees
		}
	}
}
</script>
