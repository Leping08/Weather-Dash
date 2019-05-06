<template>
	<div class="bg-grey-darkest font-sans leading-normal tracking-normal">
		<div class="mx-auto">
			<div class="flex content-center justify-center">
				<div class="w-2/3 shadow-lg p-2 bg-black m-2">
					<cluster />
				</div>
				<div class="w-1/3 shadow-lg p-2 bg-black m-2 content-center">
					<sun />
				</div>
			</div>
			<div class="flex">
				<div class="w-1/2 shadow-lg p-2 bg-black m-2">
					<pressure />
				</div>
				<div class="w-1/2 shadow-lg p-2 bg-black m-2">
					<temperature />
				</div>
			</div>
		</div>
	</div>
</template>

<script>
import pressure from '~/layouts/line-chart/pressure.vue'
import temperature from '~/layouts/line-chart/temperature.vue'
import cluster from '~/layouts/gauge-cluster.vue'
import sun from '~/layouts/time.vue'
export default {
	components: {
		pressure,
		temperature,
		cluster,
		sun
	},
	asyncData(context) {
		// called every time before loading the component
		// as the name said, it can be async
		// Also, the returned object will be merged with your data object
		return {}
	}
}
</script>
