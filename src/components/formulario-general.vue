<template>
	<form>
		<div class="card m-3">
			<div class="card-header">
				<div class="d-flex align-items-center justify-content-between">
					<label style="margin: 0px 15px;" for="año">Creacion de medios {{ msg }}</label>
					<input ref="año" type="number" name="año" id="año" class="form-control"
						style="margin: 5px 0px; width: 10%; min-width: 100px;" placeholder="Año" value=2023
						inputmode="numeric" required>
				</div>
			</div>
			<div class="card-body">
				<h5 class="card-title">Instrucciones</h5>
				<p class="card-text">Cargue los documentos en formato ___ tanto terceros como balance de prueba</p>

				<label for="terceros-path" class="form-label mb-2">Terceros:</label>
				<input class="form-control" type="file" id="terceros-path" ref="terceros" placeholder="terceros.txt"
					required @change="guardarArchivo">

				<label for="balance-path" class="form-label mb-2">Balance de prueba:</label>
				<input class="form-control" type="file" id="balance-path" ref="balance" placeholder="balance.txt" required>

				<div class="d-flex my-2">
					<button type="button" class="btn btn-primary w-50" title="Ejecutar" @click="recorrerArchivo">
						Ejecutar
						<i class="bi bi-check2-circle m-1"></i>
					</button>
					<button type="button" class="btn btn-warning w-25 mx-2" title="descargar" @click="generarArchivo">
						<i class="bi bi-download m-1"></i>
					</button>
					<button type="reset" class="btn btn-danger w-25" title="Limpiar">
						<i class="bi bi-stars m-1"></i>
					</button>
				</div>
				<p class="text-danger-emphasis bg-danger-subtle border border-danger-subtle" v-show="error" style="text-align: center;">Hubo un error cargando el archivo. ¿Seguro que lo seleccionaste?</p>

			</div>
		</div>
	</form>
</template>

<script>
export default {
	name: 'f-gerneral',
	props: {
		msg: String
	},
	methods: {
		guardarArchivo(evento) {
			console.log(evento);
			this.archivo = evento.target.files[0];
		},
		recorrerArchivo() {
			try {
				const lector = new FileReader();
				lector.readAsText(this.archivo);
				lector.onload = () => {
					const contenido = lector.result;
					const lineas = contenido.split('\n')
					console.log(lineas.length);
					lineas.forEach(linea => {
						this.terceros.push(linea)
						console.log(linea);
					});
				};
				this.error=false
				console.log('pasoooo');
			} catch (error) {
				this.error=true
				console.error(error);
				
			}
		},
		generarArchivo() {

		}
	},
	data() {
		return {
			terceros: new Array(),
			archivo: null,
			error: false,
		}
	}
};
</script>

<style scoped></style>