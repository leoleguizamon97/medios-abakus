<template>
	<div>
		<formulario msg="Distritales"/>
		<tabla/>
	</div>

</template>

<script>
import formulario from './formulario-general.vue';
import tabla from './resultados-general.vue'
export default {
	name: 'm-distritales',
	props: {
		msg: String
	},
	components: {
		formulario,
		tabla
	}
}
</script>

<style scoped></style>
