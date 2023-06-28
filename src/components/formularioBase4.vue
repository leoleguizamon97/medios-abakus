<template>
	<label for="input-group" class="form-label">Bases de cuenta: {{ msg }}</label>
	<div class="flex-column" id="base4s">
		<div class="input-group mb-3">
			<span class="input-group-text" id="cuenta01">Cuenta -01</span>
			<input type="number"  ref= "base401" id="base401" class="form-control" defaultValue="0.19" placeholder="Tarifa cuenta 01" aria-label="base01" step="0.01" required>
		</div>
		<div class="input-group mb-3">
			<span class="input-group-text" id="cuenta02">Cuenta -02</span>
			<input type="number"  ref= "base402" id="base402" class="form-control" defaultValue="0.19" placeholder="Tarifa cuenta 02" aria-label="base02" step="0.01" required>
		</div>
		<div class="input-group mb-3">
			<span class="input-group-text" id="cuenta03">Cuenta -03</span>
			<input type="number"  ref= "base403" id="base403" class="form-control" defaultValue="0.19" placeholder="Tarifa cuenta 03" aria-label="base03" step="0.01" required>
		</div>
		<div class="input-group mb-3">
			<span class="input-group-text" id="cuenta04">Cuenta -04</span>
			<input type="number"  ref= "base404" id="base404" class="form-control" defaultValue="0.19" placeholder="Tarifa cuenta 04" aria-label="base04" step="0.01" required>
		</div>
		<div class="input-group mb-3">
			<span class="input-group-text" id="cuenta05">Cuenta -05</span>
			<input type="number"  ref= "base405" id="base405" class="form-control" defaultValue="0.19" placeholder="Tarifa cuenta 05" aria-label="base05" step="0.01" required>
		</div>
		<div class="input-group mb-3">
			<span class="input-group-text" id="cuenta06">Cuenta -06</span>
			<input type="number"  ref= "base406" id="base406" class="form-control" defaultValue="0.19" placeholder="Tarifa cuenta 06" aria-label="base06" step="0.01" required>
		</div>
		<div class="input-group mb-3">
			<span class="input-group-text" id="cuenta07">Cuenta -07</span>
			<input type="number"  ref= "base407" id="base407" class="form-control" defaultValue="0.19" placeholder="Tarifa cuenta 07" aria-label="base07" step="0.01" required>
		</div>
		<div class="input-group mb-3">
			<span class="input-group-text" id="cuenta08">Cuenta -08</span>
			<input type="number"  ref= "base408" id="base408" class="form-control" defaultValue="0.19" placeholder="Tarifa cuenta 08" aria-label="base08" step="0.01" required>
		</div>
		<div class="input-group mb-3">
			<span class="input-group-text" id="cuenta09">Cuenta -09</span>
			<input type="number"  ref= "base409" id="base409" class="form-control" defaultValue="0.19" placeholder="Tarifa cuenta 09" aria-label="base09" step="0.01" required>
		</div>
		<div class="input-group mb-3">
			<span class="input-group-text" id="cuenta10">Cuenta -10</span>
			<input type="number"  ref= "base410" id="base410" class="form-control" defaultValue="6,9" placeholder="Tarifa cuenta 10" aria-label="base10" step="0.01" required>
		</div>
		<div class="input-group mb-3">
			<span class="input-group-text" id="cuenta15">Cuenta -15</span>
			<input type="number"  ref= "base415" id="base415" class="form-control" defaultValue="9,66" placeholder="Tarifa cuenta 15" aria-label="base15" step="0.01" required>
		</div>
		<div class="input-group mb-3">
			<span class="input-group-text" id="cuenta20">Cuenta -20</span>
			<input type="number"  ref= "base420" id="base420" class="form-control" defaultValue="11,04" placeholder="Tarifa cuenta 20" aria-label="base20" step="0.01" required>
		</div>
	</div>
</template>

<script>
export default {
	name: 'basesA46',
	props: {
		msg: String
	},
	methods: {
	},

	data() {
		return {
		}
	},
	components: {
	}
};
</script>

<style scoped></style>