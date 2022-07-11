<template>
	<div class="home">
		<div class="featured-card">
			<router-link to="/movie/tt4189022">
				<img src="https://m.media-amazon.com/images/M/MV5BMTYyMjQyNTE5MF5BMl5BanBnXkFtZTgwMjEyMjE2NDM@._V1_SX300.jpg"
					alt="Ash vs Evil Dead" class="featured-img">
				<div class="detail">
					<h3>Ash vs Evil Dead</h3>
					<p>Lorem ipsum dolor sit, amet consectetur adipisicing elit. Eaque aut distinctio sequi, laboriosam non libero
						animi soluta optio numquam architecto quo voluptas sint et nihil praesentium id perferendis maiores sit.</p>
				</div>
			</router-link>
		</div>

		<form @submit.prevent="SearchMovies" class="search-box">
			<input type="text" placeholder="What movie are you search?" v-model="search">
			<button type="submit">Search</button>
		</form>

		<div class="movies-list">
			<div class="movie" v-for="movie in movies" :key="movie.imdbID">

				<router-link :to="'/movie/'+movie.imdbID" class="movie-link">
					<div class="movie-img">
						<img :src="movie.Poster" :alt="movie.Title">
						<div class="type">{{movie.Type}}</div>
					</div>
					<div class="detail">
						<p class="year">{{movie.Year}}</p>
						<h3>{{movie.Title}}</h3>
					</div>
				</router-link>

			</div>
		</div>

	</div>
</template>

<script>

import { ref } from 'vue';
import env from '@/env.js';

export default {
	setup() {
		const search = ref('');
		const movies = ref([]);

		const SearchMovies = () => {
			if (search.value != '') {
				fetch(`https://www.omdbapi.com/?apikey=${env.apikey}&s=${search.value}`)
					.then(response => response.json())
					.then(data => {
						movies.value = data.Search;
						search.value = '';
						console.log(movies.value);
					});
			}
		}

		return {
			search,
			movies,
			SearchMovies
		}
	}
}
</script>

<style lang="scss">
.home {
	.featured-card {
		position: relative;
		.featured-img {
			display: block;
			width: 100%;
			height: 300px;
			object-fit: cover;
			object-position: top;
			position: relative;
		}
		.detail {
			position: absolute;
			bottom:0;
			left: 0;
			right:0;
			background: rgba(0,0,0,0.6);
			padding: 16px;
			h3 {
				color:#fff;
				margin-bottom: 16px;
			}
			p {
				color:#fff;
			}
		}
	}
	.search-box {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		padding: 15px;

		input,
		button {
			display: block;
			border:none;
			outline: none;
			background: none;
			appearance: none;
		}
		input {
			width: 100%;
			color:#fff;
			font-size: 20px;
			padding: 10px 15px;
			background: #496583;
			border-radius: 8px;
			margin-bottom: 15px;
			transition: .4s;
			&::placeholder {
				color:#f3f3f3;
			}
			&:focus {
				box-shadow: 0 3px 6px rgba(0,0,0,0.2);
			}
		}
		button {
			width: 100%;
			max-width: 300px;
			background: #42b883;
			padding: 16px;
			border-radius: 8px;
			font-size: 20px;
			color:#fff;
			text-transform: uppercase;
			transition: .4s;
			&:active {
				background: #3b8070;
			}
		}
	}
	.movies-list {
		display: flex;
		flex-wrap: wrap;
		margin: 0 8px;
		.movie {
			max-width: 50%;
			flex: 1 1 50%;
			padding: 15px 8px;
			.movie-link {
				display: flex;
				flex-direction: column;
				height: 100%;

				.movie-img {
					position: relative;
					display: block;
					img {
						display: block;
						width: 100%;
						min-height: 275px;
						object-fit: cover;
						object-position: top;
					}
					.type {
						position: absolute;
						padding: 8px 15px;
						background: #42b883;
						color:#fff;
						bottom: 15px;
						left:0;
						text-transform: capitalize;
					}
				}
			}
		.detail {
			background: #496583;
			padding: 15px 8px;
			flex: 1 1 100%;
			border-radius: 0 0 8px 8px;

			.year {
				color:#aaa;
				font-size: 14px;
			}
			h3 {
				color: #fff;
				font-weight: bold;
				font-size: 18px;
			}
		}
		}
	}
}

</style>