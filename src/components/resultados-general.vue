<template>
	<div class="card m-3 ">
		<div class="card-header" style="text-align: center;">
			« Resultados »
		</div>
		<table class="table h-100">
			<thead>
				<tr>
					<th scope="col">#</th>
					<th scope="col">First</th>
					<th scope="col">Last</th>
					<th scope="col">Handle</th>
				</tr>
			</thead>
			<tbody>
				<tr>
					<th scope="row">1</th>
					<td>Mark</td>
					<td>Otto</td>
					<td>@mdo</td>
				</tr>
			</tbody>
		</table>
	</div>
	
</template>

<script>
export default {
	name: 'resultados-generales',
	props: {
		msg: String
	},
}
</script>

<style scoped>
</style>
