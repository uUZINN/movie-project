<template>
    <header id="header" role="banner">
        <div class="header__img">
            <img :src="selectedBanner" alt="">
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
                <img :src="selectedPoster" alt="">
            </div>
            <div class="movie-desc">
                <h2 class="movie-info__title">{{ selectedMovie.title }}</h2>
                <p class="movie-info__overview">{{ selectedMovie.overview }}</p>
                <p class="movie-info__release-date">개봉일 : {{ selectedMovie.releaseDate }}</p>
                <p class="movie-info__vote-count">평점 : {{ selectedMovie.averageRating }}</p>
                <div class="characters">
                    <div v-for="character in selectedMovie.characters" :key="character.characterName" class="character">
                        <img :src="character.characterImage" :alt="character.characterName">
                        <p>{{ character.characterName }}</p>
                    </div>
                </div>
            </div>

        </div>
    </header>
    <main id="main">
        <DetailIntro v-if="movieBasic" :movieBasic="movieBasic" />
        <DetailInfo v-if="movieInfo" :movieInfo="movieInfo" />
        <DetailReview v-if="movieReview" :movieReview="movieReview" />
        <DetailCredits :movieId="movieId" v-if="movieCredits" :movieCredits="movieCredits" />
    </main>
</template>

<script>
import { onMounted, ref } from "vue";
import { useRoute } from "vue-router";
import axios from 'axios';

import DetailIntro from "../components/detail/DetailIntro.vue"
import DetailInfo from "../components/detail/DetailInfo.vue"
import DetailReview from "../components/detail/DetailReview.vue"
import DetailCredits from "../components/detail/DetailCredits.vue"


export default {
    data() {
        return {
            apiKey: '76b685aa4703f3886166774eb859abf6',
            language: 'ko-KR',
            selectedBanner: '',
            selectedMovie: {
                title: '',
                overview: '',
                releaseDate: '',
                averageRating: '',
            },
            selectedPoster: null
        };
    },
    created() {
        this.fetchMovieDetails(this.movieId);
        this.fetchBannerImage();
    },
    methods: {
        async fetchBannerImage() {
            try {
                const bannerResponse = await axios.get(`https://image.tmdb.org/t/p/original/${movie.backdrop_path}`); // 배너 이미지를 가져오는 API 호출
                this.selectedBanner = bannerResponse.data.bannerImageUrl; // 가져온 배너 이미지 URL을 selectedBanner 변수에 할당
            } catch (error) {
                console.error('Error fetching banner image:', error);
            }
        },
        async fetchMovieDetails(movieId) {
            try {
                const response = await axios.get(
                    `https://api.themoviedb.org/3/movie/${movieId}?api_key=${this.apiKey}&language=${this.language}`
                );

                const selectedMovie = response.data;

                // 선택된 영화 정보 업데이트
                this.selectedMovie.title = selectedMovie.title;
                this.selectedMovie.overview = selectedMovie.overview;
                this.selectedMovie.releaseDate = selectedMovie.release_date;
                this.selectedMovie.averageRating = selectedMovie.vote_average;

                // 선택된 포스터 업데이트
                this.selectedPoster = `https://image.tmdb.org/t/p/original/${selectedMovie.poster_path}`;
            } catch (error) {
                console.error('Error fetching movie details:', error);
            }
        }
    },


    name: "MovieDetailPage",

    props: {
        movieInfo: Object,
        movieReview: Object,
        movieCredits: Object,
        movieId: {
            type: String,
            required: true
        },
    },

    components: {
        DetailIntro,
        DetailInfo,
        DetailReview,
        DetailCredits,
    },
    setup(props) {
        const movieBasic = ref(null);
        const movieInfo = ref(null);
        const movieReview = ref(null);
        const movieCredits = ref(null);
        const route = useRoute();
        const movieId = props.movieId;


        onMounted(async () => {
            const apiKey = import.meta.env.VITE_API_KEY;
            const language = "ko-KR";

            try {
                const resMovieBasic = await axios.get(`https://api.themoviedb.org/3/movie/${movieId}?language=${language}&api_key=${apiKey}`);
                movieBasic.value = resMovieBasic.data;
                console.log(resMovieBasic.data)

                const resMovieInfo = await axios.get(`https://api.themoviedb.org/3/movie/${movieId}?language=${language}&api_key=${apiKey}`);
                movieInfo.value = resMovieInfo.data;
                console.log(resMovieInfo.data)

                const resMovieReview = await axios.get(`https://api.themoviedb.org/3/movie/${movieId}/reviews?language=${language}&api_key=${apiKey}`);
                movieReview.value = resMovieReview.data;
                console.log(resMovieReview.data)

                const resMovieCredits = await axios.get(`https://api.themoviedb.org/3/movie/${movieId}/credits?language=${language}&api_key=${apiKey}`);
                movieCredits.value = resMovieCredits.data.cast;
                console.log(resMovieCredits.data.cast)

            } catch (err) {
                console.log(err)
            }


        });



        return { movieBasic, movieCredits };

    }


}
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