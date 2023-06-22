<template>
	<label for="input-group" class="form-label">Bases de cuenta: {{ msg }}</label>
	<div class="flex-column" id="base6s">
		<div class="input-group mb-3">
			<span class="input-group-text" id="cuenta01">Cuenta -01</span>
			<input type="number"  ref= "base601" id="base601" class="form-control" placeholder="0.19" aria-label="base01" defaultValue="0.19" step="0.01" required>
		</div>
		<div class="input-group mb-3">
			<span class="input-group-text" id="cuenta02">Cuenta -02</span>
			<input type="number"  ref= "base602" id="base602" class="form-control" placeholder="0.19" aria-label="base02" defaultValue="0.19" step="0.01" required>
		</div>
		<div class="input-group mb-3">
			<span class="input-group-text" id="cuenta03">Cuenta -03</span>
			<input type="number"  ref= "base603" id="base603" class="form-control" placeholder="0.19" aria-label="base03" defaultValue="0.19" step="0.01" required>
		</div>
		<div class="input-group mb-3">
			<span class="input-group-text" id="cuenta04">Cuenta -04</span>
			<input type="number"  ref= "base604" id="base604" class="form-control" placeholder="0.19" aria-label="base04" defaultValue="0.19" step="0.01" required>
		</div>
		<div class="input-group mb-3">
			<span class="input-group-text" id="cuenta05">Cuenta -05</span>
			<input type="number"  ref= "base605" id="base605" class="form-control" placeholder="0.19" aria-label="base05" defaultValue="0.19" step="0.01" required>
		</div>
		<div class="input-group mb-3">
			<span class="input-group-text" id="cuenta06">Cuenta -06</span>
			<input type="number"  ref= "base606" id="base606" class="form-control" placeholder="0.19" aria-label="base06" defaultValue="0.19" step="0.01" required>
		</div>
		<div class="input-group mb-3">
			<span class="input-group-text" id="cuenta07">Cuenta -07</span>
			<input type="number"  ref= "base607" id="base607" class="form-control" placeholder="0.19" aria-label="base07" defaultValue="0.19" step="0.01" required>
		</div>
		<div class="input-group mb-3">
			<span class="input-group-text" id="cuenta08">Cuenta -08</span>
			<input type="number"  ref= "base608" id="base608" class="form-control" placeholder="0.19" aria-label="base08" defaultValue="0.19" step="0.01" required>
		</div>
		<div class="input-group mb-3">
			<span class="input-group-text" id="cuenta06">Cuenta -09</span>
			<input type="number"  ref= "base609" id="base609" class="form-control" placeholder="0.19" aria-label="base09" defaultValue="0.19" step="0.01" required>
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