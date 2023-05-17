<template>
	<div class="card m-3">
		<div class="card-header">
			<div class="d-flex align-items-center justify-content-between">
				<label style="margin: 0px 15px;" for="año">Creacion de medios {{ msg }}</label>
				<input type="number" name="año" id="año" class="form-control"
					style="margin: 5px 0px; width: 10%; min-width: 100px;" placeholder="2023" required>
			</div>
		</div>
		<div class="card-body">
			<h5 class="card-title">Instrucciones</h5>
			<p class="card-text">Cargue los documentos en formato ___ tanto terceros como balance de prueba</p>
			<form>
				<label for="terceros-path" class="form-label mb-2">Terceros:</label>
				<input class="form-control" type="file" id="terceros-path">
				<label for="balance-path" class="form-label mb-2">Balance de prueba:</label>
				<input class="form-control" type="file" id="bp-path">
				<div class="d-flex my-2">
					<button type="submit" class="btn btn-primary w-50">Ejecutar</button>
					<button type="submit" class="btn btn-warning w-25 mx-2">Descargar</button>
					<button type="reset" class="btn btn-danger w-25">Limpiar</button>
				</div>
				
			</form>
		</div>
	</div>
</template>

<script>
export default {
	name: 'f-gerneral',
	props: {
		msg: String
	}
}
</script>

<style scoped></style>