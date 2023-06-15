<template>
	<form id="formulario" @submit.prevent="principal">
		<div class="card m-2">
			<div class="card-header">
				<div class="d-flex align-items-center justify-content-between">
					<label style="margin: 0px 15px;" for="año">Creacion de medios {{ msg }}</label>
					<input ref="año" type="number" name="año" id="año" class="form-control"
						style="margin: 5px 0px; width: 10%; min-width: 100px;" placeholder="Año" defaultValue="2023"
						inputmode="numeric" required>
				</div>
			</div>
			<div class="card-body">
				<h5 class="card-title text-center">Instrucciones</h5>
				<p class="card-text">Cargue los documentos en formato txt o csv. Tanto terceros como balance de prueba</p>
				<div class="w-100">
					<label for="terceros-path" class="form-label">Terceros:</label>
					<input class="form-control" type="file" id="terceros-path" ref="terceros" placeholder="terceros.txt"
						required @change="guardarPathTerceros">
					<label for="balance-path" class="form-label">Balance de prueba:</label>
					<input class="form-control" type="file" id="balance-path" ref="balance" placeholder="balance.txt"
						required @change="guardarPathBalance">
				</div>
				<div class="w-100 m-1" style="text-align: center;">Articulos</div>
				<div class="btn-group m-1 nav" role="group" aria-label="articulos">
					<input required type="radio" class="nav-link btn-check form-check-input" data-bs-toggle="tab"
						data-bs-target="#base2" name="btnradio" id="btnradio1" @click="setArticulo(2)" disabled>
					<label class="btn btn-outline-secondary" for="btnradio1">Art 2</label>
					<input required type="radio" class="nav-link btn-check form-check-input" data-bs-toggle="tab"
						data-bs-target="#base3" name="btnradio" id="btnradio2" @click="setArticulo(3)" disabled>
					<label class="btn btn-outline-secondary" for="btnradio2">Art 3</label>
					<input required type="radio" class="nav-link btn-check form-check-input" data-bs-toggle="tab"
						data-bs-target="#base4" name="btnradio" id="btnradio3" @click="setArticulo(4)">
					<label class="btn btn-outline-secondary" for="btnradio3">Art 4</label>
					<input required type="radio" class="nav-link btn-check form-check-input" data-bs-toggle="tab"
						data-bs-target="#base6" name="btnradio" id="btnradio4" @click="setArticulo(6)">
					<label class="btn btn-outline-secondary" for="btnradio4">Art 6</label>
				</div>
				<div id="Bases" class="tab-content">
					<div class="tab-pane fade" id="base2" aria-labelledby="btnradio1" tabindex="1">
						<bases2 msg="otros" />
					</div>
					<div class="tab-pane fade" id="base4" aria-labelledby="btnradio2" tabindex="2">
						<bases msg="2368" />
					</div>
					<div class="tab-pane fade" id="base6" aria-labelledby="btnradio3" tabindex="3">
						<bases msg="135518" />
					</div>
				</div>
				<div class="d-flex my-2">
					<button type="submit" class="btn btn-primary w-50" title="Ejecutar">
						Ejecutar
						<i class="bi bi-check2-circle m-1"></i>
					</button>
					<button type="button" class="btn btn-warning w-25 mx-2" title="descargar"
						@click="generarArchivo(this.b46, 'desdePC')">
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
import bases2 from './formularioBases2.vue'

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
				this.terceros.forEach(
					persona => { persona[0]=persona[0].replaceAll(' ','')
					
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
				//Limpiar balance
				this.balance.pop();		//Ultimo elemento
				this.balance.shift();	//Primer elemento
				var head = '';
				var btemp=new Map();
				for (let index = 0; index < this.balance.length; index++) {
					const element = this.balance[index];
					if(element[0]==''){
						//Valida si ya existe la llave
						if(!btemp.has(head)){
							btemp.set(head,new Array()) //Crea una array vacia en donde se ingresaran las lineas relevantes
							//console.log('Cabecera: '+head+' creada');
						}
						btemp.get(head).push(element)
					}else{
						var fila = element[0].split(' ');
						head = fila[0];
						//console.log(head+' <-Esta es la cabecera actual')
					}
				}
				this.b46 = btemp;
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
						if (inicial == 0) {
							nombreEmpresa = linea.replaceAll(';', '')
							console.log('Nombre de empresa: ' + nombreEmpresa);
							inicial += 1;
						} else if (linea.replaceAll(';', '') == nombreEmpresa) {
							console.log('Se omitio esta linea:- ' + inicial + linea);
							inicial = 1
						} else if (inicial != -1) {
							console.log('Se omitio esta linea: ' + inicial + linea);
							inicial += 1;
							if (inicial == 4) {
								inicial = -1;
							}
						} else {
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
			if (this.articulo == 2) {
				console.log('Se ejecutara el articulo 2');
			} else if (this.articulo == 3) {
				console.log('Se ejecutara el articulo 3');
			} else if (this.articulo == 4) {
				console.log('Se ejecutara el articulo 4');
			}
		},
		//Creacion de articulos
		
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
			art4: new Array(),

			//Balance para art 4 y 6
			b46: new Array(),

			//Files
			archivoTerceros: null,
			archivoBalance: null,
			error: false,
		}
	},
	components: {
		tabla,
		bases,
		bases2,
	}
};
</script>

<style scoped></style>