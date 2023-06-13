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
					<div class="btn-group-vertical w-25 m-1" role="group" aria-label="articulos" required>
						<div class="w-100" style="text-align: center;">Articulos</div>
						<input type="radio" class="btn-check" name="btnradio" id="btnradio1" autocomplete="off" @click="setArticulo(2)" checked>
						<label class="btn btn-outline-secondary" for="btnradio1">Art 2</label>
						<input type="radio" class="btn-check" name="btnradio" id="btnradio2" autocomplete="off"	@click="setArticulo(3)">
						<label class="btn btn-outline-secondary" for="btnradio2">Art 3</label>
						<input type="radio" class="btn-check" name="btnradio" id="btnradio3" autocomplete="off"	@click="setArticulo(4)">
						<label class="btn btn-outline-secondary" for="btnradio3">Art 4</label>
					</div>
				</div>
				<div id="Contenido" class="tab-content">
					<div class="tab-pane fade show active" id="bases" role="tabpanel" aria-labelledby="nav-Distritales" tabindex="0">
						<bases/>
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
	<tabla />
</template>

<script>
import tabla from './resultados-general.vue'
import bases from './formularioBases.vue'

export default {

	name: 'f-gerneral',
	props: {
		msg: String
	},
	methods: {
		//Obtencion informacion de form
		setArticulo(value) {
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
			this.personas = new Array();
			this.balance = new Array();
			this.cuentas = new Array();
		},
		crearTerceros() {
			return new Promise(resolve => {
				this.personas.forEach(p => {
					//console.log(p);
					var persona = new Array()
					p.split(';').forEach(
						atributo => { persona.push(atributo) }
					);
					this.terceros.push(persona)
				});
				resolve('resolved')
			})
		},
		crearBalance() {
			return new Promise(resolve => {
				this.cuentas.forEach(p => {
					var cuenta = new Array()
					p.split(';').forEach(
						atributo => { cuenta.push(atributo) }
					);
					this.balance.push(cuenta)
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
						this.personas.push(linea)
					})
					console.log('resultado' + this.personas);
					resolve('Cargado')
				}
				lector.onerror = reject
			});
		},
		cargarArchivoBalance() {
			return new Promise((resolve, reject) => {
				const lector = new FileReader();
				lector.readAsText(this.archivoBalance);
				lector.onload = () => {
					const lineas = lector.result.split('\n');
					var nombreEmpresa = "";
					var inicial = 0;
					console.log(lineas.length + '<- Cantidad de lineas Balance');

					lineas.forEach(linea => {
						var lineatemp = linea.replace(/\s+/g, '')
						if(inicial==0){
							nombreEmpresa = lineatemp;
							//console.log('Nombre de empresa: '+nombreEmpresa);
							inicial+=1;
						}else if(linea.replace(/\s+/g, '') == nombreEmpresa){
							//console.log('Se omitio esta linea:- '+inicial+linea);
							inicial = 1
						}else if(inicial != -1){
							//console.log('Se omitio esta linea: '+inicial+linea);
							inicial+=1;
							if(inicial == 4){
								inicial= -1;
							}
						}else{
							this.cuentas.push(linea)
						}
					})

					console.log('resultado' + this.cuentas);
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
			this.cargarArchivoBalance().then(() => {
				this.crearBalance();
			})
			//Decide cual articulo realizar
			if(this.articulo == 2){
				console.log('Se ejecutara el articulo 2');
			}else if(this.articulo == 3){
				console.log('Se ejecutara el articulo 3');
			}else if(this.articulo == 4){
				console.log('Se ejecutara el articulo 4');
			}
		},
	},

	data() {
		return {
			articulo: 2,
			//Temporales
			personas: new Array(),
			cuentas: new Array(),
			//Finales
			terceros: new Array(),
			balance: new Array(),

			archivoTerceros: null,
			archivoBalance: null,
			error: false,
		}
	},

	components: {
		tabla,
		bases,
	}
};
</script>

<style scoped></style>