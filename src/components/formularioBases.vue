<template>
	<label for="input-group" class="form-label">Bases de cuenta: {{ msg }}</label>
	<div class="flex-column" id="Bases">
		<div class="input-group mb-3">
			<span class="input-group-text" id="cuenta01">Cuenta -01</span>
			<input type="number"  ref= "base01" id="base01" class="form-control" placeholder="0.19" aria-label="base01" defaultValue="0.19" step="0.01" required>
		</div>
		<div class="input-group mb-3">
			<span class="input-group-text" id="cuenta02">Cuenta -02</span>
			<input type="number"  ref= "base02" id="base02" class="form-control" placeholder="0.19" aria-label="base02" defaultValue="0.19" step="0.01" required>
		</div>
		<div class="input-group mb-3">
			<span class="input-group-text" id="cuenta03">Cuenta -03</span>
			<input type="number"  ref= "base03" id="base03" class="form-control" placeholder="0.19" aria-label="base03" defaultValue="0.19" step="0.01" required>
		</div>
		<div class="input-group mb-3">
			<span class="input-group-text" id="cuenta04">Cuenta -04</span>
			<input type="number"  ref= "base04" id="base04" class="form-control" placeholder="0.19" aria-label="base04" defaultValue="0.19" step="0.01" required>
		</div>
		<div class="input-group mb-3">
			<span class="input-group-text" id="cuenta05">Cuenta -05</span>
			<input type="number"  ref= "base05" id="base05" class="form-control" placeholder="0.19" aria-label="base05" defaultValue="0.19" step="0.01" required>
		</div>
		<div class="input-group mb-3">
			<span class="input-group-text" id="cuenta06">Cuenta -06</span>
			<input type="number"  ref= "base06" id="base06" class="form-control" placeholder="0.19" aria-label="base06" defaultValue="0.19" step="0.01" required>
		</div>
		<div class="input-group mb-3">
			<span class="input-group-text" id="cuenta07">Cuenta -07</span>
			<input type="number"  ref= "base07" id="base07" class="form-control" placeholder="0.19" aria-label="base07" defaultValue="0.19" step="0.01" required>
		</div>
		<div class="input-group mb-3">
			<span class="input-group-text" id="cuenta08">Cuenta -08</span>
			<input type="number"  ref= "base08" id="base08" class="form-control" placeholder="0.19" aria-label="base08" defaultValue="0.19" step="0.01" required>
		</div>
		<div class="input-group mb-3">
			<span class="input-group-text" id="cuenta06">Cuenta -09</span>
			<input type="number"  ref= "base09" id="base09" class="form-control" placeholder="0.19" aria-label="base09" defaultValue="0.19" step="0.01" required>
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