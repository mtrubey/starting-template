<template>
	<div id="app">

		<app-header :changeSearch="changeSearch"/>

		<div class="container">
			<h1 class="pt-3 pb-3">Персонажи Marvel</h1>


			<h5 v-if="!character.length && !loading">nothing!!!</h5>

<!--			<pre>APP search:{{search}}</pre>
			<pre>character:{{character}}</pre>-->

			<app-modal :character="characters[characterIndex]" />

			<spinner v-if="loading"/>

			<div class="row">
				<div v-for="(el, index) in character" :key="el.id" class="card mb-3 col-sm-12 col-md-6 col-lg-4" style="max-width: 540px;">
					<div class="row g-0">
						<div class="col-md-4">
							<img :src="el.thumbnail"
							     class="img-fluid rounded-start" :alt="el.name">
						</div>
						<div class="col-md-8">
							<div class="card-body">
								<h5 class="card-title">{{ el.name }}</h5>
								<!-- Button trigger modal -->
								<button type="button" class="btn btn-secondary btn-sm" data-bs-toggle="modal"
								        data-bs-target="#exampleModal" @click="characterIndex = index">
									Подробнее
								</button>
							</div>
						</div>
					</div>
				</div>
			</div>
		</div>

	</div>
</template>

<script>
import Spinner from "./components/Spinner";
import AppModal from "./components/AppModal";
import AppHeader from "./components/AppHeader";

export default {
	name: 'App',
	components: {
		AppHeader,
		AppModal,
		Spinner,
	},
	data() {
		return {
			loading: false,
			characters: [],
			characterIndex: 0,
			search: '',
		}
	},
	methods: {
		fetchCharacters: function () {
			return fetch('https://netology-api-marvel.herokuapp.com/characters')
				.then(res => res.json())
				.then(json => this.characters = json);
		},
		changeSearch: function (value) {
			this.search = value;
		},
	},
	computed: {
		character: function () {
			const {characters, search} = this;
			return characters.filter((character) => {
				return character.name.toLowerCase().indexOf(search.toLowerCase()) !== -1
			})
		}
	},
	async mounted() {
		this.loading = true;
		await this.fetchCharacters();
		this.loading = false;
	}
}
</script>

<style>

</style>
