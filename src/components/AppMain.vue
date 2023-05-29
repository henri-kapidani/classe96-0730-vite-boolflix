<script>
import ResultsList from './ResultsList.vue';
import { store } from '../store';

export default {
	data() {
		return {
			store,
		};
	},
	components: {
		ResultsList,
	},
};
</script>

<template>
	<ResultsList
		listTitle="Risultati dei film"
		:arrCards="store.arrMovies"
		:arrCategories="store.arrMoviesCategories"
		cardType="movie"
	/>

	<ResultsList
		listTitle="Risultati delle serie TV"
		:arrCards="store.arrSeries"
		:arrCategories="store.arrSeriesCategories"
		cardType="tv"
	/>
</template>

<style></style>
