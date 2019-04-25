<template>
	<div>
		<h1>Dashboard</h1>
		<no-ssr>
			<apex-chart
				width="1000"
				type="line"
				:options="options"
				:series="series"
			></apex-chart>
		</no-ssr>
	</div>
</template>

<script>
export default {
	asyncData(context) {
		// called every time before loading the component
		// as the name said, it can be async
		// Also, the returned object will be merged with your data object
		return {
			options: {
				chart: {
					height: 500,
					type: 'line',
					animations: {
						enabled: true,
						easing: 'linear',
						dynamicAnimation: {
							speed: 2000
						}
					},
					toolbar: {
						show: true
					},
					zoom: {
						enabled: true
					}
				}
			},
			series: [
				{
					name: 'Temp',
					data: [30, 40, 45, 50, 49]
				},
				{
					name: 'Humidity',
					data: [40, 30, 50, 60, 70]
				}
			],
			dataLabels: {
				enabled: false
			},
			stroke: {
				curve: 'smooth'
			},
			title: {
				text: 'Dynamic Updating Chart',
				align: 'left'
			},
			markers: {
				size: 0
			},
			xaxis: {
				type: 'datetime',
				range: 777600000
			},
			yaxis: {
				max: 100
			},
			legend: {
				show: true
			}
		}
	}
}
</script>

<style>
.red {
	color: red;
}
</style>
