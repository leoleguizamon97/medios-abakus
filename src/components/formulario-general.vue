<template>
	<form id="formulario" @submit.prevent="principal">
		<div class="card m-2 ">
			<div class="card-header">
				<div class="d-flex align-items-center justify-content-between">
					<label style="margin: 0px 15px;" for="año">Creacion de medios {{ msg }}</label>
					<input ref="año" type="number" name="año" id="año" class="form-control"
						style="margin: 5px 0px; width: 10%; min-width: 100px;" placeholder="Año" defaultValue="2022"
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
						data-bs-target="#base2" name="btnradio" id="btnradio1" @click="setArticulo(2)">
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
						<bases4 msg="236801" />
					</div>
					<div class="tab-pane fade" id="base6" aria-labelledby="btnradio3" tabindex="3">
						<bases6 msg="135518" />
					</div>
				</div>
				<div class="d-flex my-2">
					<button type="submit" class="btn btn-primary w-50" title="Ejecutar">
						Ejecutar
						<i class="bi bi-check2-circle m-1"></i>
					</button>
					<button type="button" class="btn btn-warning w-25 mx-2" title="descargar" @click="vigente()">
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
import bases4 from './formularioBase4.vue'
import bases6 from './formularioBase6.vue'
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
			this.terceros = new Map();
			this.personas = new Array();

			this.balance = new Array();
			this.cuentas = new Array();
			this.b46 = new Map();

			this.art4 = new Array();
			this.art6 = new Array();
			this.art2 = new Array();

			this.cuentasBuscar = new Array();
			this.cuentasExcluir = new Array();

			this.empresaData = new Array();

		},
		crearTerceros() {
			return new Promise(resolve => {
				var tempTerceros = new Array();
				this.personas.forEach(p => {
					//console.log(p);
					var persona = new Array()
					p.split(';').forEach(
						atributo => { persona.push(atributo) }
					);
					tempTerceros.push(persona)
				});
				tempTerceros.forEach(
					persona => {
						persona[0] = persona[0].replaceAll(' ', '')

					});
				tempTerceros.forEach(element => {
					var id = element.shift()
					var datos = element
					this.terceros.set(id, datos)
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
					//console.log(cuenta[3] + '???');
					this.balance.push(cuenta)

				});
				//Limpiar balance
				console.log(this.balance.pop() + '------ -');		//Ultimo elemento
				console.log(this.balance.pop() + '------ -');		//Ultimo elemento
				this.balance.shift();								//Primer elemento
				this.balance.forEach(linea => {
					linea[5] = linea[5].replaceAll(',', '.');
					linea[8] = linea[8].replaceAll(',', '.');
					linea[10] = linea[10].replaceAll(',', '.');
					linea[14] = linea[14].replaceAll(',', '.');
				});
				//Crear balance b46
				var head = '';
				var btemp = new Map();
				for (let index = 0; index < this.balance.length; index++) {
					const element = this.balance[index];
					if (element[0] == '') {
						//Valida si ya existe la llave
						if (!btemp.has(head)) {
							btemp.set(head, new Array()) //Crea una array vacia en donde se ingresaran las lineas relevantes
							//console.log('Cabecera: '+head+' creada');
						}
						var id = element[3].split('-');
						id[0] = id[0].replace(/\D/g, '')
						//console.log(head,id[0]);
						element[3] = id[0]
						if (id[0] == '899999960' || id[0] == '899999061'|| id[0] == '899999861') {
							console.log('deleted');
						} else {
							btemp.get(head).push(element)
						}
					} else {
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
				const nombre = filen + '-' + this.empresaData[1] + '.txt'
				enlace.setAttribute(
					"href",
					"data:text/plain;charset=utf-18," + encodeURIComponent(contenido)
				);
				enlace.setAttribute("download", nombre);
				enlace.style.display = "none";
				document.body.appendChild(enlace);
				enlace.click();
				document.body.removeChild(enlace);
				resolve('resolved')
			})
		},
		vigente() {
			if (this.articulo == 4) {
				this.generarArchivo(this.art4, 'Articulo4')
				return 0;
			} else if (this.articulo == 6) {
				this.generarArchivo(this.art6, 'Articulo6')
				return 0;
			} else if (this.articulo == 2) {
				this.generarArchivo(this.art2, 'Articulo2')
				return 0;
			}
		},
		obtenerDiferencia(arr1, arr2) {
			const diferencia = arr1.filter(elemento => !arr2.includes(elemento));
			return diferencia;
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
					//console.log('resultado' + this.personas);
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
							//console.log('Nombre de empresa: ' + nombreEmpresa);
							inicial += 1;
						} else if (inicial == 1) {
							console.log(linea.split('-')[0].replace(/\D/g, '') + 'Este es el id de la empresa');
							this.idEmpresa = linea.split('-')[0].replace(/\D/g, '')
							inicial += 1;
						} else if (linea.replaceAll(';', '') == nombreEmpresa) {
							//console.log('Se omitio esta linea:- ' + inicial + linea);
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
					//console.log('resultado' + this.cuentas);
					resolve('Cargado')
				}
				lector.onerror = reject
			});
		},
		//Creacion de articulo
		empresa(id) {
			this.empresaData.push(id)
			this.empresaData.push(this.buscarPersona(id)[2])
			this.empresaData.push(this.buscarPersona(id)[3])
		},
		buscarCuenta(cuenta, mapa) {
			const cuentas = [];
			//Busca en estas llaves
			for (var key of mapa) {
				if (key[0].startsWith(cuenta)) {
					cuentas.push(key[0]);
					//console.log(key[0] + 'Test');
				}
			}
			return cuentas;
		},
		buscarPersona(id) {
			var persona = this.terceros.get(id)
			return persona
		},
		crearCabecera(art) {
			var cabecera = Array()
			cabecera.push('')
			cabecera.push('VIGENCIA')
			cabecera.push('TIPO DOCUMENTO')
			cabecera.push('NUMERO DOCUMENTO')
			cabecera.push('NOMBRE O RAZON SOCIAL')
			cabecera.push('DIRECCION DE NOTIFICACION')
			cabecera.push('TELEFONO')
			cabecera.push('E-MAIL')
			cabecera.push('CODIGO MUNICIPIO')
			cabecera.push('CODIGO DEPARTAMENTO')
			if (art == 2) {
				cabecera.push('CONCEPTO PAGO O ABONO EN CUENTA')
				cabecera.push('VALOR COMPRAS ANUAL')
				cabecera.push('VALOR DEVOLUCIONES')
			} else if (art == 4) {
				cabecera.push('BASE RETENCION')
				cabecera.push('TARIFA RETENCION APLICADA')
				cabecera.push('MONTO RETENCION ANUAL')
			} else if (art == 6) {
				cabecera.push('MONTO PAGO')
				cabecera.push('TARIFA RETENCION APLICADA')
				cabecera.push('MONTO RETENCION ANUAL')
			}

			return cabecera
		},
		tipoDocumento(tipo) {
			switch (tipo) {
				case "A":
					tipo = "NIT";
					break;
				case "C":
					tipo = "CC";
					break;
				case "E":
					tipo = "CE";
					break;
				case "P":
					tipo = "PA";
					break;
				case "T":
					tipo = "TI";
					break;
				default:
					tipo = "DESCONOCIDO";
					break;
			}
			return tipo
		},
		articulo2() {
			this.cuentasBuscar = ['5', '6', '7', '14', '15'];
			this.cuentasExcluir = ['54', '53', '5160', '5299', '5205', '5105', '7201','511570']
			var cuentasA2 = new Array();
			//Ingreso
			this.cuentasBuscar.forEach(a => {
				var cuentas = this.buscarCuenta(a, this.b46)
				if (cuentas.length == 0) {
					console.log('Cuenta vacia: ' + a)
				} else {
					cuentasA2 = cuentasA2.concat(cuentas)
				}
			});
			//Excluir
			this.cuentasExcluir.forEach(a => {
				var cuentas = this.buscarCuenta(a, this.b46)
				if (cuentas.length == 0) {
					console.log('Cuenta vacia: ' + a)
				} else {
					cuentasA2 = this.obtenerDiferencia(cuentasA2, cuentas)
				}
			});
			//Obtener lineas
			var lineas = new Array();
			cuentasA2.forEach(res => {
				var valores = this.b46.get(res);
				valores.forEach(linea => {
					linea.push(res.substring(res.length - 2))
					lineas.push(linea);
				});
			});
			//Cabecera
			//this.art2.push(this.crearCabecera(2));
			//Obtener valores acumulados
			var acumulado = new Map();
			lineas.forEach(mov => {
				if (!acumulado.has(mov[3])) {
					//Si es nuevo
					acumulado.set(mov[3], [parseInt(mov[8]), parseInt(mov[10]),mov[3]])
					console.log(mov[3] + ' Nuevo');
				} else {
					//Si ya existe
					console.log(mov[3] + ' Viejo');
					var valoresAnteriores = acumulado.get(mov[3])
					acumulado.set(
						mov[3],
						[parseInt(mov[8]) + valoresAnteriores[0]
						,parseInt(mov[10]) + valoresAnteriores[1]
						,mov[3]]
					)
				}
			});
			console.log(acumulado);
			acumulado.forEach(mov => {
				var final = Array();
				//Buscamos el tercero
				if (parseFloat(mov[0]) - parseFloat(mov[1]) >= (38004 * 70)) {
					var tercero = this.buscarPersona(mov[2])
					console.log(mov[3]);
					//Definimos tipo de documento
					var tipo = this.tipoDocumento(tercero[1])
					//direccion
					var direccion = '';
					if (tercero[3] == '') {
						direccion = this.empresaData[2];
					} else {
						direccion = tercero[3];
					}
					//Telefono
					var telefono = '';
					if (tercero[4] == '') {
						telefono = '0';
					} else {
						telefono = tercero[4];
					}
					//correo
					var correo = '';
					if (tercero[9] == '') {
						correo = 'NA';
					} else {
						correo = tercero[9];
					}
					//Se crea linea de informe
					final.push('\n')
					final.push(document.getElementById('año').value)
					final.push(tipo)
					final.push(mov[2])
					final.push(tercero[2])
					final.push(direccion)
					final.push(telefono)
					final.push(correo)
					final.push(tercero[6])
					final.push(tercero[6].substring(0, 2))
					final.push('1')
					final.push(parseInt(mov[0]))
					final.push(parseInt(mov[1]))
					//console.log(final + ' FINAL ');
					this.art2.push(final);
				}
			})
		},
		articulo4() {
			return new Promise((resolve) => {
				var cuenta = this.buscarCuenta('236805', this.b46);
				//obtener lineas de articulo
				var lineas = new Array();
				cuenta.forEach(res => {
					var valores = this.b46.get(res);
					valores.forEach(linea => {
						linea.push(res.substring(res.length - 2))
						console.log(linea);
						lineas.push(linea);
					});

				});
				//crear articulo
				//cabecera
				//this.art4.push(this.crearCabecera(4));
				lineas.forEach(mov => {
					var final = Array();
					//Buscamos el tercero
					if (parseFloat(mov[10]) - parseFloat(mov[8]) > 0) {
						var tercero = this.buscarPersona(mov[3])
						//Definimos tipo de documento
						var tipo = this.tipoDocumento(tercero[1])
						//definimos la base
						var tarifa = mov[mov.length - 1]
						switch (tarifa) {
							case "01":
								tarifa = document.getElementById('base401').value
								break;
							case "02":
								tarifa = document.getElementById('base402').value
								break;
							case "03":
								tarifa = document.getElementById('base403').value
								break;
							case "04":
								tarifa = document.getElementById('base404').value
								break;
							case "05":
								tarifa = document.getElementById('base405').value
								break;
							case "06":
								tarifa = document.getElementById('base406').value
								break;
							case "07":
								tarifa = document.getElementById('base407').value
								break;
							case "08":
								tarifa = document.getElementById('base408').value
								break;
							case "09":
								tarifa = document.getElementById('base409').value
								break;
							case "10":
								tarifa = document.getElementById('base410').value
								break;
							case "15":
								tarifa = document.getElementById('base415').value
								break;
							case "20":
								tarifa = document.getElementById('base420').value
								break;
							default:
								tarifa = "DESCONOCIDO"
								break;
						}
						//direccion
						var direccion = '';
						if (tercero[3] == '') {
							direccion = this.empresaData[2];
						} else {
							direccion = tercero[3];
						}
						//Telefono
						var telefono = '';
						if (tercero[4] == '') {
							telefono = '0';
						} else {
							telefono = tercero[4];
						}
						//correo
						var correo = '';
						if (tercero[9] == '') {
							correo = 'NA';
						} else {
							correo = tercero[9];
						}
						//definimos la tarifa
						var base = (parseFloat(mov[10]) - parseFloat(mov[8])) / parseFloat(tarifa) * 1000
						//Se crea linea de informe
						final.push('\n')
						final.push(document.getElementById('año').value)
						final.push(tipo)
						final.push(mov[3])
						final.push(tercero[2])
						final.push(direccion)
						final.push(telefono)
						final.push(correo)
						final.push(tercero[6])
						final.push(tercero[6].substring(0, 2))
						final.push(parseInt(base))
						final.push(tarifa)
						final.push(parseInt(parseFloat(mov[10]) - parseFloat(mov[8])))
						console.log(final);
						this.art4.push(final);
					}
				});
				//console.log(lineas);
				resolve('resolved')
			})
		},
		articulo6() {
			return new Promise((resolve) => {
				var cuenta = this.buscarCuenta('135518', this.b46);
				//obtener lineas de articulo
				var lineas = new Array();
				cuenta.forEach(res => {
					var valores = this.b46.get(res);
					valores.forEach(linea => {
						linea.push(res.substring(res.length - 2))
						console.log(linea);
						lineas.push(linea);
					});

				});
				//crear articulo
				//cabecera
				//this.art6.push(this.crearCabecera(6));
				lineas.forEach(mov => {
					var final = Array();
					//Buscamos el tercero
					if (parseFloat(mov[8]) - parseFloat(mov[10]) > 0) {
						var tercero = this.buscarPersona(mov[3])
						console.log(mov[3]);
						//Definimos tipo de documento
						var tipo = this.tipoDocumento(tercero[1])
						//definimos la base
						var tarifa = mov[mov.length - 1]
						switch (tarifa) {
							case "01":
								tarifa = document.getElementById('base601').value
								break;
							case "02":
								tarifa = document.getElementById('base602').value
								break;
							case "03":
								tarifa = document.getElementById('base603').value
								break;
							case "04":
								tarifa = document.getElementById('base604').value
								break;
							case "05":
								tarifa = document.getElementById('base605').value
								break;
							case "06":
								tarifa = document.getElementById('base606').value
								break;
							case "07":
								tarifa = document.getElementById('base607').value
								break;
							case "08":
								tarifa = document.getElementById('base608').value
								break;
							case "09":
								tarifa = document.getElementById('base609').value
								break;
							case "10":
								tarifa = document.getElementById('base610').value
								break;
							default:
								tarifa = "DESCONOCIDO"
								break;
						}
						//direccion
						var direccion = '';
						if (tercero[3] == '') {
							direccion = this.empresaData[2];
						} else {
							direccion = tercero[3];
						}
						//Telefono
						var telefono = '';
						if (tercero[4] == '') {
							telefono = '0';
						} else {
							telefono = tercero[4];
						}
						//correo
						var correo = '';
						if (tercero[9] == '') {
							correo = 'NA';
						} else {
							correo = tercero[9];
						}
						//definimos la tarifa
						var base = (parseFloat(mov[8]) - parseFloat(mov[10])) / parseFloat(tarifa) * 1000
						//Se crea linea de informe
						final.push('\n')
						final.push(document.getElementById('año').value)
						final.push(tipo)
						final.push(mov[3])
						final.push(tercero[2])
						final.push(direccion)
						final.push(telefono)
						final.push(correo)
						final.push(tercero[6])
						final.push(tercero[6].substring(0, 2))
						final.push(parseInt(base))
						final.push(tarifa)
						final.push(parseInt(parseFloat(mov[8]) - parseFloat(mov[10])))
						console.log(final);
						this.art6.push(final);
					}
				});
				//console.log(lineas);
				resolve('resolved')
			})
		},
		//MAin
		principal() {
			//Decide cual articulo realizar
			this.limpiar()
			this.cargarArchivoTerceros()
				.then(() => this.crearTerceros())
				.then(() => this.cargarArchivoBalance())
				.then(() => this.crearBalance())
				.then(() => {
					this.empresa(this.idEmpresa)
					if (this.articulo == 2) {
						console.log('Se ejecutara el articulo 2');
						this.articulo2()
					} else if (this.articulo == 4) {
						console.log('Se ejecutara el articulo 4');
						this.articulo4()
					} else if (this.articulo == 6) {
						console.log('Se ejecutara el articulo 6');
						this.articulo6()
					}
				})
		},
	},
	data() {
		return {
			//Temporales
			personas: new Array(),
			cuentas: new Array(),
			empresaData: new Array(),
			idEmpresa: '0',
			//Finales
			terceros: new Map(),
			balance: new Array(),
			art4: new Array(),
			art6: new Array(),
			art2: new Array(),
			//Balance para art 4 , 6 y 2
			b46: new Map(),
			//Cuentas a buscar
			cuentasBuscar: new Array(),
			cuentasExcluir: new Array(),
			//Files
			archivoTerceros: null,
			archivoBalance: null,
			error: false,
		}
	},
	components: {
		tabla,
		bases4,
		bases2,
		bases6
	}
};
</script>

<style scoped></style>