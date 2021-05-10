<template>
    <main>
        <ul v-for="film in filmsArray" :key="film.id">
            <li>
                <img
                    v-if="film.backdrop_path || film.poster_path"
                    :src="
                        `https://image.tmdb.org/t/p/w342${
                            film.poster_path === null
                                ? film.backdrop_path
                                : film.poster_path
                        }`
                    "
                    :alt="`${film.title} poster`"
                />
                <span v-else>/////No poster/////</span>
            </li>
            <li>Title: {{ film.title || film.name }}</li>
            <li>Original-Title: {{ film.original_title || film.original_name }}</li>
            <li class="language">
                Language:
                <img
                    v-if="flagsArray.includes(film.original_language)"
                    :src="require(`../assets/imges/${film.original_language}.png`)"
                    :alt="film.original_language"
                />
                <span v-else>{{ film.original_language }}</span>
            </li>
            <li>Vote: {{ Math.ceil(film.vote_average / 2) }}</li>
        </ul>
    </main>
</template>

<script>
export default {
    name: "Main",
    props: {
        filmsArray: Array,
    },
    data() {
        return {
            flagsArray: ["en", "it"],
        };
    },
};
</script>

<style scoped lang="scss">
main {
    .language img {
        width: 25px;
        height: 15px;
        vertical-align: middle;
    }
    ul {
        padding-left: 25px;
        li:last-child {
            margin-bottom: 25px;
        }
    }
}
</style>
