<script>
import LangFlag from 'vue-lang-code-flags/LangFlag.vue';
import { store } from '../store';
import axios from 'axios';

export default {
	data() {
		return {
			store,
			arrActors: [],
			arrGenres: [],
		};
	},
	props: {
		cardData: Object,
	},
	components: {
		LangFlag,
	},
	created() {
		// generi
		axios
			.get(
				`https://api.themoviedb.org/3/${this.cardData.type}/${this.cardData.id}`,
				{
					params: {
						api_key: this.store.apiKey,
					},
				}
			)
			.then(response => {
				console.log('genere', response.data.genres);
				this.arrGenres = response.data.genres;
			});

		// attori
		axios
			.get(
				`https://api.themoviedb.org/3/${this.cardData.type}/${this.cardData.id}/credits`,
				{
					params: {
						api_key: this.store.apiKey,
					},
				}
			)
			.then(response => {
				response.data.cast.splice(5);
				console.log('attori', response.data.cast);
				this.arrActors = response.data.cast;
			});
	},
};
</script>

<template>
	<div>
		<img :src="cardData.image" :alt="cardData.title" />
		<ul>
			<li>{{ cardData.title }}</li>
			<li>{{ cardData.originalTitle }}</li>
			<li>
				<LangFlag :iso="cardData.language" :squared="false" />
				<span class="lang-text">{{ cardData.language }}</span>
			</li>
			<li>
				<font-awesome-icon
					v-for="i in cardData.rating"
					:key="i"
					icon="fa-solid fa-star"
					class="star"
				/>

				<font-awesome-icon
					v-for="i in 5 - cardData.rating"
					:key="i"
					icon="fa-regular fa-star"
					class="star"
				/>
			</li>
			<li>
				Genres:
				<ol>
					<li v-for="genre in arrGenres" :key="genre.id">
						{{ genre.name }}
					</li>
				</ol>
			</li>
			<li>
				Actors:
				<ol>
					<li v-for="actor in arrActors" :key="actor.id">
						{{ actor.name }}
					</li>
				</ol>
			</li>
		</ul>
	</div>
</template>

<style scoped>
img {
	width: 100%;
}

.lang-text {
	display: none;
}

.flag-icon-undefined {
	display: none;
}

.flag-icon-undefined + .lang-text {
	display: inline;
}

.star {
	color: salmon;
}
</style>
