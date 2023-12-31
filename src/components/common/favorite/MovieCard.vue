<template>
    <div class="moviecard">
        <div class="cover" @mouseover="isHovered = true" @mouseout="isHovered = false">
            <button class="favorite-button" @click="removeFavorite(movie)">
                <font-awesome-icon :icon="['fas', 'heart']" />
            </button>
            <img :src="'https://image.tmdb.org/t/p/w500' + movie.poster_path" :alt="movie.title" />
            <img :src="'https://image.tmdb.org/t/p/w500' + movie.poster_path" :alt="movie.title" />
            <div class="texts" :class="{ 'overview-visible': isHovered }">
                <p>출시일 : {{ movie.release_date || movie.first_air_date }}</p>
                <p>관객수 : {{ movie.popularity || movie.first_air_date }}</p>
                <p>평점 : {{ movie.vote_average }}
                    <span v-for="star in starRatings" :key="star" class="star"
                        :class="{ filled: star <= movie.vote_average }">★</span>
                </p>
                <p>내용 : {{ movie.overview || movie.title || movie.name }}</p>
            </div>
        </div>
        <div class="script">
            <h2>{{ movie.title || movie.name }}</h2>

            <div class="star-ratings">
                <span v-for="star in starRatings" :key="star" class="star"
                    :class="{ filled: star <= movie.vote_average }">★</span>
            </div>
        </div>
    </div>
</template>

<script>
import { useStore } from 'vuex';
import { ref } from 'vue';

export default {
    props: {
        movie: {
            type: Object,
            required: true,
        },
    },
    computed: {
        starRatings() {
            const ratings = Math.round(this.movie.vote_average / 2);
            return Array.from({ length: 5 }, (_, index) => index + 1).filter((star) => star <= ratings);
        },
    },
    setup(props) {
        const store = useStore();
        const isHovered = ref(false);
        const removeFavorite = (movie) => {
            store.dispatch('removeFavorite', movie);
        };

        return {
            isHovered,
            removeFavorite,
        }
    }
};

</script>


<style lang="scss" scoped>
.moviecard {
    width: 100%;
    color: white;

    .cover {
        margin-top: 30px;
        margin-bottom: 10px;
        z-index: 10;
        position: relative;
        transition: transform 0.3s ease-in-out;
        color: #ffffff00;
        text-align: left;
        overflow: hidden;
        border-radius: 20px;
        width: 100%;
        padding-bottom: 150%;

        &:hover {
            color: #ffffff;
            transform: translateY(-30px);
            transition-delay: 0.3s;
        }
    }

    img {
        position: absolute;
        background-size: cover;
        width: 100%;
        transition: transform 0.3s ease-in-out;

        &:hover {
            transition: all 0.3s ease-in-out;
            filter: brightness(0) invert(0);
            opacity: 0.7;
        }
    }

    .texts {
        position: absolute;
        display: -webkit-box;
        -webkit-box-orient: vertical;
        -webkit-line-clamp: 10;
        margin: 10px;
        overflow: hidden;
        text-overflow: ellipsis;
        word-break: break-all;
        font-size: 12px;

    }
}

.star-ratings {
    display: flex;
    justify-content: center;
    margin-bottom: 10px;

    .star {
        color: #FFD700;
        font-size: 16px;
        transition: color 0.3s ease-in-out;

        &.filled {
            color: #FFCC00;
        }
    }
}

.favorite-button {
    position: absolute;
    top: 10px;
    right: 10px;
    z-index: 990;
    border: none;
    cursor: pointer;
    font-size: 30px;
    display: flex;
    align-items: center;
    transition: transform 0.2s ease-in-out;
    color: red;

    :hover {
        color: gray;

    }

}

.favorite-button:hover {
    transform: scale(1.1);
}
</style>