<template>
	<label for="input-group" class="form-label">Elementos excluidos Art 2</label>
	<div class="input-group mb-3">
		<input type="number" class="form-control" placeholder="Cuenta a eliminar" aria-label="cuentaEliminar"
			aria-describedby="button-addon2">
		<button class="btn btn-primary" type="button" id="button-addon2">
			<i class="bi bi-plus-circle"></i>
		</button>
		<ul class="list-group list-group-flush ">

		</ul>

	</div>
</template>

<script>
export default {
	name: 'basesA2',
	props: {
		msg: String
	},
	methods: {
	},

	data() {
		return {
			cuentasIgnorar: ['54','53','5160','5299','5205','5105'],
		}
	},

	components: {
	}
};
</script>

<style scoped></style>