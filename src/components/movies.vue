<template>
    <div v-if="showMovies" class="buttons">
        <button @click="randomizeMovie">Randomize a movie to watch</button>
        <button @click="increaseContainer">+</button>
        <button @click="decreaseContainer">-</button>

    </div>
    <div v-if="showMovies" class="movie-wrapper">
        <div class="movie-container" :style="{ width: widthOnContainer }" v-for="(movie) in moviesData" :key="movie.id">
            <h1 class="movie-title">{{ movie.name }}</h1>
            <img class="movie-image" :src="movie.image" alt="Movie Image">
            <p>IMDB {{ movie.aggregateRating.ratingValue }}</p>
        </div>
    </div>

    <div v-if="!showMovies" class="buttons">
        <button @click="randomizeMovie">Randomize a new movie</button>
        <button @click="back">Back to all movies</button>
    </div>
    <div v-if="!showMovies" class="movie-wrapper">
        <div class="movie-container">
            <h1 class="movie-title">{{ randomMovie.name }}</h1>
            <img class="movie-image" :src="randomMovie.image" alt="Movie Image">
            <p>IMDB {{ randomMovie.aggregateRating.ratingValue }}</p>
        </div>
    </div>



</template>

<script setup>
import { ref } from "vue";

const moviesData = ref([]);
const widthOnContainer = ref("400px");
const showMovies = ref(true);
const randomMovie = ref(null);

function randomizeMovie() {

    const randomIndex = Math.floor(Math.random() * moviesData.value.length);
    randomMovie.value = moviesData.value[randomIndex];
    console.log("Random Movie:", randomMovie.value);
    showMovies.value = false;
}
function back() {

    showMovies.value = true;
    randomMovie.value = null;
}
function increaseContainer() {

    widthOnContainer.value = parseInt(widthOnContainer.value) + 25 + "px";
    console.log(widthOnContainer.value);

}

function decreaseContainer() {
    if (parseInt(widthOnContainer.value) > 25) {
        widthOnContainer.value = parseInt(widthOnContainer.value) - 25 + "px";
        console.log(widthOnContainer.value);
    } else {
        console.log("Container width cannot be less than 0px.");
    }
}


function getMovies() {
    fetch(
        "https://raw.githubusercontent.com/theapache64/top250/master/top250.json"
    )
        .then((res) => res.json())
        .then((data) => {
            moviesData.value = data;
            console.log(data);
        })
        .catch((error) => {
            console.error("Error fetching movies:", error);
        });
}

getMovies();
</script>

<style scoped>
button {

    background-color: rgb(238, 161, 18);
    color: white;
    border: none;
    margin: 10px;
    padding: 10px 20px;
    cursor: pointer;
    border-radius: 5px;
    transition: background-color 0.3s;
}

button:hover {
    background-color: rgb(200, 140, 0);
}

.movie-title {
    max-width: 100%;
    margin: 10px;
    font-style: normal;
    font-weight: bold;
    color: rgb(238, 161, 18);
    text-shadow: 4px 5px 5px black;
}

.movie-wrapper {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    align-items: center;
    text-align: center;
}

.movie-container {
    border: solid black 2px;
    background-color: rgb(33, 31, 46);
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    text-align: center;
    padding: 10px;
    margin: 10px;
}

.movie-image {
    max-width: 100%;
    height: auto;
    object-fit: cover;
}

.buttons {
    display: flex;
    text-align: center;
    align-items: center;
    justify-content: center;
}
</style>
