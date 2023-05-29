<script>
import CardComponent from './CardComponent.vue';

export default {
	data() {
		return {
			idGenreFilter: '',
		};
	},
	props: {
		listTitle: String,
		arrCategories: Array,
		arrCards: Array,
		cardType: String,
	},
	components: {
		CardComponent,
	},
	methods: {
		formatObj(originalObj) {
			switch (this.cardType) {
				case 'movie':
					return this.formatObjMovies(originalObj);
					break;
				case 'tv':
					return this.formatObjSerie(originalObj);
					break;
			}
		},
		formatObjMovies(originalObj) {
			return {
				id: originalObj.id,
				title: originalObj.title,
				originalTitle: originalObj.original_title,
				language: originalObj.original_language,
				rating: this.convertRating(originalObj.vote_average, 10, 5),
				image: this.makeUrl(originalObj.poster_path),
				type: this.cardType,
			};
		},
		formatObjSerie(originalObj) {
			return {
				id: originalObj.id,
				title: originalObj.name,
				originalTitle: originalObj.original_name,
				language: originalObj.original_language,
				rating: this.convertRating(originalObj.vote_average, 10, 5),
				image: this.makeUrl(originalObj.poster_path),
				type: this.cardType,
			};
		},
		convertRating(originalRating, originalMax, newMax) {
			//originalRating : 10 = newRating : 5
			return Math.round((originalRating * newMax) / originalMax);
		},
		makeUrl(partialPath) {
			return partialPath
				? 'https://image.tmdb.org/t/p/w342' + partialPath
				: '/img/default-poster.jpg';
		},
	},
	computed: {
		arrFiltered() {
			return this.idGenreFilter
				? this.arrCards.filter(element =>
						element.genre_ids.includes(this.idGenreFilter)
				  )
				: this.arrCards;
		},
	},
};
</script>

<template>
	<section>
		<div class="heading">
			<h2>{{ listTitle }}</h2>
			<select v-model="idGenreFilter">
				<option value="">All Categories</option>
				<option
					v-for="category in this.arrCategories"
					:key="category.id"
					:value="category.id"
				>
					{{ category.name }}
				</option>
			</select>
		</div>

		<div class="results">
			<CardComponent
				v-for="card in arrFiltered"
				:key="card.id"
				:cardData="formatObj(card)"
				class="card"
			/>
		</div>
	</section>
</template>

<style>
.heading {
	display: flex;
	justify-content: space-between;
	align-items: center;
}

.results {
	display: flex;
	flex-wrap: wrap;
}

.card {
	flex: 0 0 33.33333%;
	border: 2px solid salmon;
}
</style>
