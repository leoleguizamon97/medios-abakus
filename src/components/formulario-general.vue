<template>
	<form id="formulario" @submit.prevent="principal">
		<div class="card m-2">
			<div class="card-header">
				<div class="d-flex align-items-center justify-content-between">
					<label style="margin: 0px 15px;" for="año">Creacion de medios {{ msg }}</label>
					<input ref="año" type="number" name="año" id="año" class="form-control"
						style="margin: 5px 0px; width: 10%; min-width: 100px;" placeholder="Año" value="2023"
						inputmode="numeric" required>
				</div>
			</div>
			<div class="card-body">
				<h5 class="card-title">Instrucciones</h5>
				<p class="card-text">Cargue los documentos en formato ___ tanto terceros como balance de prueba</p>
				<div class="d-flex">
					<div class="w-75">
						<label for="terceros-path" class="form-label">Terceros:</label>
						<input class="form-control" type="file" id="terceros-path" ref="terceros" placeholder="terceros.txt"
							required @change="guardarPathTerceros">
						<label for="balance-path" class="form-label">Balance de prueba:</label>
						<input class="form-control" type="file" id="balance-path" ref="balance" placeholder="balance.txt"
							required @change="guardarPathBalance">
					</div>
						<div class="btn-group-vertical ms-1 w-25" role="group" aria-label="articulos">
							<div class="w-100" style="text-align: center;">Articulos</div>
							<input type="radio" class="btn-check" name="btnradio" id="btnradio1" autocomplete="off" checked @click="setArticulo(1)">
							<label class="btn btn-outline-secondary" for="btnradio1">123</label>
							<input type="radio" class="btn-check" name="btnradio" id="btnradio2" autocomplete="off" @click="setArticulo(2)">
							<label class="btn btn-outline-secondary" for="btnradio2">123</label>
							<input type="radio" class="btn-check" name="btnradio" id="btnradio3" autocomplete="off" @click="setArticulo(3)">
							<label class="btn btn-outline-secondary" for="btnradio3">123</label>
						</div>
				</div>

				<div class="d-flex my-2">
					<button type="submit" class="btn btn-primary w-50" title="Ejecutar">
						Ejecutar
						<i class="bi bi-check2-circle m-1"></i>
					</button>
					<button type="button" class="btn btn-warning w-25 mx-2" title="descargar"
						@click="generarArchivo(this.terceros, 'desdePC')">
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
		setArticulo(value){
			this.articulo = value;
		},
		guardarPathTerceros(evento) {
			this.archivoTerceros = evento.target.files[0];
		},
		guardarPathBalance(evento) {
			this.archivoBalance = evento.target.files[0];
		},
		//Manejo de informacion
		limpiar() {
			this.terceros = new Array();
			this.balance = new Array();
		},
		crearTerceros() {
			return new Promise(resolve => {
				console.log('ENTRO')
				console.log(this.terceros)
				this.terceros.forEach(p => {
					console.log(p);
					var persona = new Array()
					p.split(';').forEach(
						atributo => { persona.push(atributo) }
					);
					this.personas.push(persona)
				});
				resolve('resolved')
			})
		},
		generarArchivo(contenido, filen) {
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
		//Cargar documentos
		cargarArchivoTerceros() {
			return new Promise((resolve, reject) => {
				const lector = new FileReader();
				lector.readAsText(this.archivoTerceros);
				lector.onload = () => {
					const lineas = lector.result.split('\n')
					console.log(lineas.length + '<- Cantidad de lineas terceros');
					lineas.forEach(linea => {
						this.terceros.push(linea)
					})
					console.log('resultado' + this.terceros);
					resolve('Cargado')
				}
				lector.onerror = reject
			});
		},
		principal() {
			this.limpiar()
			this.cargarArchivoTerceros().then(() => {
				this.crearTerceros();
			})
		},
	},

	data() {
		return {
			articulo: 1,
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