<template>
    <div>
        <img
            v-if="film.backdrop_path || film.poster_path"
            :src="
                `https://image.tmdb.org/t/p/w342${
                    film.poster_path === null ? film.backdrop_path : film.poster_path
                }`
            "
            :alt="`${film.title} poster`"
        />
        <span v-else>/////No poster/////</span>
        <ul>
            <li
                v-if="
                    (film.title || film.name) ==
                        (film.original_title || film.original_name)
                "
            >
                Title: {{ film.title || film.name }}
            </li>
            <template v-else>
                <li>Title: {{ film.title || film.name }}</li>
                <li>
                    Original-Title: {{ film.original_title || film.original_name }}
                </li>
            </template>
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
ul {
    padding-left: 25px;
    .language img {
        width: 25px;
        height: 15px;
        vertical-align: middle;
    }
    li:last-child {
        margin-bottom: 25px;
    }
}
</style>
