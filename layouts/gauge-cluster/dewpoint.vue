<template>
	<div class="p-4">
		<no-ssr>
			<apex-chart
				type="radialBar"
				height="300"
				:options="chartOptions"
				:series="[dewpoint]"
			/>
		</no-ssr>
	</div>
</template>

<script>
import Echo from 'laravel-echo'
export default {
	filters: {
		round: function(value) {
			return parseFloat(value).toFixed(2)
		},
		tempInCelcus: function(value) {
			return (value - 32) * (5 / 9)
		},
		tempInFahrenheit: function(value) {
			return (value * 9) / 5 + 32
		}
	},
	data() {
		return {
			temperature: 0,
			humidity: 0,
			chartOptions: {
				fill: {
					colors: ['#F6993F']
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
				labels: ['Dewpoint']
			}
		}
	},
	computed: {
		dewpoint: function() {
			return this.$options.filters.round(
				this.$options.filters.tempInFahrenheit(
					this.$options.filters.tempInCelcus(this.temperature) -
						(100 - this.humidity) / 5
				)
			)
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
					this.temperature = message.temperature.degrees
					this.updateData(message)
				})
			}
		},
		updateData(data) {
			this.humidity = data.humidity.percentage
		}
	}
}
</script>
