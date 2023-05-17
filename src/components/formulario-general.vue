<template>
	<form id="formulario" @submit.prevent="principal">
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
				<h5 class="card-title" @click="asyncCall">Instrucciones</h5>
				<p class="card-text">Cargue los documentos en formato ___ tanto terceros como balance de prueba</p>

				<label for="terceros-path" class="form-label mb-2">Terceros:</label>
				<input class="form-control" type="file" id="terceros-path" ref="terceros" placeholder="terceros.txt"
					required @change="guardarArchivoTerceros">

				<label for="balance-path" class="form-label mb-2">Balance de prueba:</label>
				<input class="form-control" type="file" id="balance-path" ref="balance" placeholder="balance.txt" required
					@change="guardarArchivoBalance">

				<div class="d-flex my-2">
					<button type="submit" class="btn btn-primary w-50" title="Ejecutar">
						Ejecutar
						<i class="bi bi-check2-circle m-1"></i>
					</button>
					<button type="button" class="btn btn-warning w-25 mx-2" title="descargar" @click="crearTerceros">
						<i class="bi bi-download m-1"></i>
					</button>
					<button type="reset" class="btn btn-danger w-25" title="Limpiar" @click="this.error = false">
						<i class="bi bi-stars m-1"></i>
					</button>
				</div>
				<p class="text-danger-emphasis bg-danger-subtle border border-danger-subtle" v-show="error"
					style="text-align: center;">Hubo un error cargando el archivo. ¿Seguro que lo seleccionaste?</p>
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
		//Obtencion informacion de form
		guardarArchivoTerceros(evento) {
			this.archivoTerceros = evento.target.files[0];
		},
		guardarArchivoBalance(evento) {
			//console.log(evento);
			this.archivoBalance = evento.target.files[0];
		},
		//Procesamiento de datos
		async recorrerArchivo() {
			return new Promise(resolve => {
				this.terceros = new Array()
				this.balance = new Array()
				try {
					const lector = new FileReader();
					lector.readAsText(this.archivoTerceros);
					lector.onload = () => {
						const contenido = lector.result;
						const lineas = contenido.split('\n')
						
						console.log(lineas.length + '<- Cantidad de lineas terceros');
						
						lineas.forEach(linea => {
							this.terceros.push(linea)
							//console.log(linea);
						});
					};
					this.error = false
				} catch (error) {
					this.error = true
					console.error(error);
				}
				try {
					const lector = new FileReader();
					lector.readAsText(this.archivoBalance);
					lector.onload = () => {
						const contenido = lector.result;
						const lineas = contenido.split('\n')
						console.log(lineas.length + '<- Cantidad de lineas balance');
						lineas.forEach(linea => {
							this.balance.push(linea)
							//console.log(linea);
						});
					};
					this.error = false
				} catch (error) {
					this.error = true
					console.error(error);
				}
				console.log(this.terceros);
				this.generarArchivo
				resolve('resolved');
			});
		},
		async crearTerceros() {
			return new Promise(resolve => {
				console.log('ENTRO')
				console.log(this.terceros[0])
				console.log(this.terceros)
				console.log(this.terceros)
				var persona = new Array()
				this.terceros[0].split(';').forEach(
						atributo => { persona.push(atributo) }
					);
					this.personas.push(persona)
				resolve('resolved')
				});
				
		},
		async generarArchivo(contenido, filen) {
			return new Promise(resolve => {
				const enlace = document.createElement("a");
				const nombre = filen + '.txt'
				enlace.setAttribute(
					"href",
					"data:text/plain;charset=utf-8," + encodeURIComponent(contenido)
				);
				enlace.setAttribute("download", nombre);
				enlace.style.display = "none";
				document.body.appendChild(enlace);
				enlace.click();
				document.body.removeChild(enlace);
				resolve('resolved')
			})
		},
		async principal() {
			this.recorrerArchivo().then(this.crearTerceros)
		},

	},

	data() {
		return {
			terceros: new Array(),
			personas: new Array(),
			balance: new Array(),
			archivoTerceros: null,
			archivoBalance: null,
			error: false,
		}
	}
};
</script>

<style scoped></style>