
    
<template>
    <header id="header" role="banner">
        <div class="header__img">
            <img :src="selectedMovie.bannerUrl" alt="">
        </div>
        <div class="header__inner">
            <h1 class="header__logo">MOVIDEO</h1>
            <div class="header__menu">
                <ul>
                    <li><a href="#">Home</a></li>
                    <li><a href="#">Series</a></li>
                    <li><a href="#">Top10</a></li>
                    <li><a href="#">코딩영화 Top10</a></li>
                </ul>
            </div>
        </div>
        <div class="movie-info">
            <div class="movie-poster">
                <img :src="selectedMovie.poster" alt="">
            </div>
            <div class="movie-desc">
                <h2 class="movie-info__title">{{ selectedMovie.title }}</h2>
                <p class="movie-info__overview">{{ selectedMovie.overview }}</p>
                <p class="movie-info__release-date">개봉일 : {{ selectedMovie.releaseDate }}</p>
                <p class="movie-info__vote-count">평점 : {{ selectedMovie.averageRating }}</p>
                <div class="characters">
                    <div class="character" v-for="character in selectedMovie.characters" :key="character.characterName">
                        <img :src="character.profilePath" :alt="character.characterName">
                        <p>{{ character.characterName }}</p>
                    </div>
                </div>
            </div>

        </div>
    </header>
</template>
<script>
import { onMounted, ref } from "vue";
import { useRoute } from "vue-router";

export default {
    setup() {
        const selectedMovie = ref({
            title: '',
            overview: '',
            releaseDate: '',
            averageRating: '',
            characters: [],
            bannerUrl: '',
            poster: '',
        });
        const route = useRoute();

        const movieInfofetch = () => {
            const movieId = route.params.movieId;
            const apiKey = import.meta.env.VITE_API_KEY;
            const url = `https://api.themoviedb.org/3/movie/${movieId}?api_key=${apiKey}&language=ko-KR`;
            const creditsUrl = `https://api.themoviedb.org/3/movie/${movieId}/credits?api_key=${apiKey}&language=ko-KR`

            Promise.all([
                fetch(url).then((response) => response.json()),
                fetch(creditsUrl).then((response) => response.json())
            ])
                .then(([movieRes, creditsRes]) => {
                    console.log(movieRes);
                    console.log(creditsRes);

                    const selectedCharacters = creditsRes.cast.slice(0, 5).map((character) => ({
                        characterName: character.character,
                        profilePath: 'https://image.tmdb.org/t/p/original/' + character.profile_path
                    }));

                    selectedMovie.value = {
                        title: movieRes.title,
                        overview: movieRes.overview,
                        releaseDate: movieRes.release_date,
                        averageRating: movieRes.vote_average,
                        bannerUrl: 'https://image.tmdb.org/t/p/original/' + movieRes.backdrop_path,
                        poster: 'https://image.tmdb.org/t/p/original/' + movieRes.poster_path,
                        characters: selectedCharacters
                    };
                })
                .catch((err) => {
                    console.error(err);
                });
        };

        onMounted(() => {
            movieInfofetch();
        });

        return {
            selectedMovie
        };
    }
};
</script>
<style lang="scss">
#header {
    height: 800px;
    position: relative;
    backdrop-filter: blur(10px);

    .header__img {
        position: absolute;
        width: 100%;
        height: 100%;
        overflow: hidden;
        z-index: -1;
    }

    .header__inner {
        display: flex;
        flex-direction: row;
        justify-content: space-between;
        align-items: center;

        max-width: 1300px;
        margin: 0 auto;
        padding: 0.5rem 0;

        h1 {
            font-weight: 900;
        }

        ul {
            display: flex;
            flex-direction: row;
            justify-content: space-between;
            align-items: center;

            li {
                margin-left: 3rem;
            }
        }
    }

    .movie-info {
        position: absolute;
        width: 100%;
        height: 100%;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
        display: flex;
        flex-direction: row;
        align-items: center;
        justify-content: space-between;
        backdrop-filter: blur(10px);
        padding: 15%;
        z-index: -1;
        background-color: #00000028;

        .movie-poster {
            width: 24%;
        }

        .movie-desc {
            width: 70%;

            h2 {
                font-size: 2.5rem;
                margin-bottom: 20px;
                font-weight: 700;
            }
        }

        .movie-info__release-date {
            margin-top: 1rem;
        }


        .characters {
            display: flex;
            flex-direction: row;
            justify-content: space-between;
            align-items: stretch;
            margin-top: 20px;

            .character {
                width: 18%;

                img {
                    height: 220px;
                    width: 100%;
                    object-fit: cover;
                }

                p {
                    margin-top: 5px;
                }
            }
        }
    }

}
</style>