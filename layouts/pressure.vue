<template>
	<div class="">
		<h1>Pressure</h1>
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
export default {
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
				grid: {
					row: {
						colors: ['#f3f3f3', 'transparent'], // takes an array which will be repeated on columns
						opacity: 0.5
					}
				},
				toolbar: {
					autoSelected: 'zoom'
				},
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
					title: {
						text: 'Millibars'
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
	},
	methods: {
		async fetchData() {
			this.initalData = await this.$axios.$get(
				'http://192.168.1.14:8000/api/pressure'
			)
			this.series[0].data = this.initalData.map(function(data) {
				return { x: Date.parse(data.measurement_time), y: data.millibars }
			})
		}
	}
}
</script>
