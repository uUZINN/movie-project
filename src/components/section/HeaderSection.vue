<script>
import axios from 'axios';

export default {
    data() {
        return {
            apiKey: '76b685aa4703f3886166774eb859abf6', // 여기에 본인의 TMDB API 키를 입력하세요
            language: 'ko-KR',
            selectedBanner: '', // 선택된 배너 이미지 URL을 저장할 변수
            selectedMovie: { // 선택된 영화 정보를 저장할 변수
                title: '',
                overview: '',
                releaseDate: '',
                averageRating: '',

            }
        };
    },
    created() {
        this.fetchPopularMovies();
    },
    methods: {

        async fetchPopularMovies() {
            try {
                const response = await axios.get(
                    `https://api.themoviedb.org/3/movie/popular?api_key=${this.apiKey}&language=ko-KR`
                );

                const popularMovies = response.data.results;
                const banners = [];

                for (const movie of popularMovies) {
                    const bannerUrl = `https://image.tmdb.org/t/p/original/${movie.backdrop_path}`;
                    const posterUrl = `https://image.tmdb.org/t/p/original/${movie.poster_path}`;

                    const creditsResponse = await axios.get(
                        `https://api.themoviedb.org/3/movie/${movie.id}/credits?api_key=${this.apiKey}&language=ko-KR`
                    );

                    const credits = creditsResponse.data;
                    const characters = credits.cast.slice(0, 5); // Selecting top 5 characters

                    const characterImages = characters.map((character) => {
                        return {
                            characterName: character.name,
                            characterImage: `https://image.tmdb.org/t/p/original/${character.profile_path}`
                        };
                    });

                    const movieInfo = {
                        id: movie.id, // 영화 ID
                        title: movie.title,
                        overview: movie.overview,
                        releaseDate: movie.release_date,
                        averageRating: movie.vote_average,
                        voteCount: movie.vote_count,
                        language: movie.language,
                        country: movie.country,
                        certification: movie.certification,
                        characters: characterImages
                    };

                    banners.push({ bannerUrl, posterUrl, movieInfo });
                }

                // Randomly select banner and update selected movie and banner
                const randomIndex = Math.floor(Math.random() * banners.length);
                this.selectedBanner = banners[randomIndex].bannerUrl;
                this.selectedPoster = banners[randomIndex].posterUrl;
                this.selectedMovie = banners[randomIndex].movieInfo;
            } catch (error) {
                console.error('Error fetching movies:', error);
            }
        }
    }
};
</script>
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
</template>

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