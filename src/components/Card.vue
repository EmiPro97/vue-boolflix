<template>
    <div class="card-wrapper">
        <img
            v-if="film.backdrop_path || film.poster_path"
            :src="
                `https://image.tmdb.org/t/p/w342${
                    film.poster_path === null ? film.backdrop_path : film.poster_path
                }`
            "
            :alt="`${film.title} poster`"
        />
        <img
            class="img-placeholder"
            v-else
            src="../assets/imges/no-img-found.jpg"
            alt="img-placeholder"
        />
        <div class="hover-card flex align-center">
            <ul>
                <li
                    v-if="
                        (film.title || film.name) ==
                            (film.original_title || film.original_name)
                    "
                >
                    <strong>Title:</strong> {{ film.title || film.name }}
                </li>
                <template v-else>
                    <li>Title: {{ film.title || film.name }}</li>
                    <li>
                        <strong>Original-Title:</strong>
                        {{ film.original_title || film.original_name }}
                    </li>
                </template>
                <li class="language">
                    <strong>Language:</strong>
                    <img
                        v-if="flagsArray.includes(film.original_language)"
                        :src="
                            require(`../assets/imges/flags/${film.original_language}.png`)
                        "
                        :alt="film.original_language"
                    />
                    <span v-else>{{ film.original_language }}</span>
                </li>
                <li>
                    Vote:
                    <i
                        v-for="(s, index) in Math.ceil(film.vote_average / 2)"
                        :key="index"
                        class="fas fa-star full"
                    ></i>
                    <i
                        v-for="(s, index) in 5 - Math.ceil(film.vote_average / 2)"
                        :key="'empty' + index"
                        class="fas fa-star empty"
                    ></i>
                </li>
                <li><strong>Overview:</strong> {{ film.overview }}</li>
            </ul>
        </div>
    </div>
</template>

<script>
export default {
    name: "Card",
    props: {
        film: Object,
    },
    data() {
        return {
            flagsArray: ["en", "it"],
        };
    },
};
</script>

<style scoped lang="scss">
// Vars
@import "../styles/vars";

.card-wrapper {
    width: 342px;
    min-height: 516.5px;
    position: relative;
    margin: 10px 3px;
    border-radius: 5px;
    overflow: hidden;
    cursor: pointer;
    transition: all 0.3s 0.5s;
    & > img {
        width: 100%;
        height: 100%;
        object-fit: cover;
        object-position: center;
    }
    .hover-card {
        opacity: 0;
        position: absolute;
        top: 0;
        bottom: 0;
        left: 0;
        right: 0;
        padding: 0 20px;
        transition: opacity 1.3s;
        background: rgba(0, 0, 0, 0.5);
        strong {
            font-size: 18px;
            margin-right: 3px;
        }
        li {
            margin-bottom: 20px;
            i.full {
                color: red;
            }
            &:last-child {
                margin-bottom: 0;
            }
        }
        .language img {
            width: 25px;
            height: 15px;
            vertical-align: middle;
        }
    }
    &:hover {
        // transition: z-index 0s 1s, transform 0.5s 0.8s;
        transform: perspective(550px) translate3d(0px, -16px, 29px);
        z-index: 2;
    }
    &:hover .hover-card {
        opacity: 1;
    }
}
</style>
