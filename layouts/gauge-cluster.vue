<template>
	<div class="">
		<div class="flex">
			<div class="w-1/3">
				<humidity />
			</div>
			<div class="w-1/3">
				<temperature />
			</div>
			<div class="w-1/3">
				<dewpoint />
			</div>
		</div>
	</div>
</template>

<script>
import humidity from '~/layouts/gauge-cluster/humidity.vue'
import temperature from '~/layouts/gauge-cluster/temperature.vue'
import dewpoint from '~/layouts/gauge-cluster/dewpoint.vue'
export default {
	components: {
		humidity,
		temperature,
		dewpoint
	},
	data() {
		return {}
	}
}
</script>
