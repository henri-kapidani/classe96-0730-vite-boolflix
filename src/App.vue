<script>
import AppHeader from './components/AppHeader.vue';
import AppMain from './components/AppMain.vue';
import axios from 'axios';
import { store } from './store';

export default {
	data() {
		return {
			store, // equivale a ----> store: store,
		};
	},
	components: {
		AppHeader,
		AppMain,
	},
	methods: {
		requestToApi(url, objParams, varResult) {
			axios
				.get(url, {
					params: objParams,
				})
				.then(response => {
					this.store[varResult] = response.data.results;
				});
		},

		searchApi(searchStr) {
			const objParams = {
				api_key: this.store.apiKey,
				query: searchStr,
			};

			this.requestToApi(
				'https://api.themoviedb.org/3/search/movie',
				objParams,
				'arrMovies'
			); // per i film

			this.requestToApi(
				'https://api.themoviedb.org/3/search/tv',
				objParams,
				'arrSeries'
			); // per le serie
		},
	},
	created() {
		// richiesta di categorie per film
		axios
			.get('https://api.themoviedb.org/3/genre/movie/list', {
				params: {
					api_key: this.store.apiKey,
				},
			})
			.then(
				response => (this.store.arrMoviesCategories = response.data.genres)
			);

		// richiesta di categorie per serie tv
		axios
			.get('https://api.themoviedb.org/3/genre/tv/list', {
				params: {
					api_key: this.store.apiKey,
				},
			})
			.then(
				response => (this.store.arrSeriesCategories = response.data.genres)
			);
	},
};
</script>

<template>
	<AppHeader @searchRequest="searchApi" />
	<AppMain />
</template>

<style>
* {
	box-sizing: border-box;
}
</style>
