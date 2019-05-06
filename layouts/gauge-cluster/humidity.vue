<template>
	<div class="p-4">
		<no-ssr>
			<apex-chart
				type="radialBar"
				height="300"
				:options="chartOptions"
				:series="[humidity]"
			/>
		</no-ssr>
	</div>
</template>

<script>
import Echo from 'laravel-echo'
export default {
	data() {
		return {
			humidity: 0,
			chartOptions: {
				fill: {
					colors: ['#6574CD']
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
								fontSize: '18px'
							}
						},
						track: {
							opacity: 0.2
						}
					}
				},
				labels: ['Humidity']
			}
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
			this.humidity = data.humidity.percentage
		}
	}
}
</script>
