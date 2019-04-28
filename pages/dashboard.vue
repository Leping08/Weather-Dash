<template>
	<div>
		<pressure />
		<temperature />
		<humidity />
	</div>
</template>

<script>
import pressure from '~/layouts/pressure.vue'
import temperature from '~/layouts/temperature.vue'
import humidity from '~/layouts/humidity.vue'
export default {
	components: {
		pressure,
		temperature,
		humidity
	},
	asyncData(context) {
		// called every time before loading the component
		// as the name said, it can be async
		// Also, the returned object will be merged with your data object
		return {}
	}
}
</script>
