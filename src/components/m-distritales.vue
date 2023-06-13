<template>
	<div>
		<formulario msg="Distritales"/>
	</div>

</template>

<script>
import formulario from './formulario-general.vue';
export default {
	name: 'm-distritales',
	props: {
		msg: String
	},
	components: {
		formulario,
	}
}
</script>

<style scoped></style>
