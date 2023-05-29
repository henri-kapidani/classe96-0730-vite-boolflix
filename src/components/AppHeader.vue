<script>
export default {
	data() {
		return {
			searchStr: '',
		};
	},
};
</script>

<template>
	<header>
		<h1>Boolflix</h1>
		<form @submit.prevent="$emit('searchRequest', searchStr)">
			<input type="text" v-model="searchStr" />
			<button>Cerca</button>
		</form>
	</header>
</template>

<style>
header {
	display: flex;
	justify-content: space-between;
	align-items: center;
	padding: 1em;
	background-color: black;
	color: white;
}
</style>
