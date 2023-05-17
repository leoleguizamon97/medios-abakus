<template>
	<div id="cabecera" class="sticky-top">
		<nav class="navbar bg-body-tertiary h-50">
			<div class="container-fluid">
				<a class="navbar-brand">
					<img src="./assets/rosa.webp" alt="A" width="30" height="30" class="rounded d-inline-block align-text-top">
					Powered by: Abakus Accounting
				</a>
			</div>
		</nav>
		<nav class="bg-body-tertiary h-50">
			<div class="nav nav-tabs" id="nav-tab" role="tablist">
				<button class="nav-link active" id="nav-Distritales" data-bs-toggle="tab" data-bs-target="#Distritales"
					type="button" role="tab" aria-controls="nav-Distritales" aria-selected="true">Distritales</button>
				<button class="nav-link" id="nav-Nacionales" data-bs-toggle="tab" data-bs-target="#Nacionales" type="button"
					role="tab" aria-controls="nav-Nacionales" aria-selected="false">Nacionales</button>
				<button class="nav-link" id="nav-Donaciones" data-bs-toggle="tab" data-bs-target="#Donaciones" type="button"
					role="tab" aria-controls="nav-Donaciones" aria-selected="false" disabled>+</button>
			</div>
		</nav>
	</div>
	<div id="Contenido" class="tab-content">
		<div class="tab-pane fade show active" id="Distritales" role="tabpanel" aria-labelledby="nav-Distritales"
			tabindex="0">
			<distritales/>
		</div>
		<div class="tab-pane fade" id="Nacionales" role="tabpanel" aria-labelledby="nav-Nacionales" tabindex="0">
			<nacionales/>
		</div>
		<div class="tab-pane fade" id="Donaciones" role="tabpanel" aria-labelledby="nav-Donaciones" tabindex="0">
			<nacionales/>
		</div>
	</div>
	<div id="footer" class="nav justify-content-center bg-body-tertiary fixed-bottom">
		<a class="nav-tabContent navbar-brand" href="https://github.com/leoleguizamon97">
			<img class="rounded" alt="ALLG" src="./assets/rosa.webp" width="15" height="15" >
			anlleguizamongu
		</a>
	</div>
</template>

<script>
import distritales from './components/m-distritales.vue';
import nacionales from './components/m-nacionales.vue';
export default {
	name: 'App',
	components: {
		distritales,
		nacionales,
	}
}
</script>

<style></style>
