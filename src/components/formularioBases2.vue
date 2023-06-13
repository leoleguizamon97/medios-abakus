<template>
	<label for="input-group" class="form-label">OTROS: {{ msg }}</label>
	<div class="flex-column" id="Bases">
		<div class="input-group mb-3">
			<span class="input-group-text" id="cuenta01">Cuenta -0555</span>
			<input type="number" class="form-control" placeholder="0.19" aria-label="base01" defaultValue="0.19" step="0.01" required>
		</div>

	</div>
</template>

<script>
export default {
	name: 'basesA2',
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