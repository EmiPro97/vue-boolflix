<template>
    <div>
        <Header @apiCallEmitted="apiCall" />
        <main>
            <ul>
                <li
                    @apiCallEmitted="apiCall"
                    v-for="film in filmsArray"
                    :key="film.id"
                    @click="test"
                >
                    Title: {{ film.title }} <br />
                    Original-Title: {{ film.original_title }} <br />
                    Language: {{ film.original_language }} <br />
                    Vote: {{ film.vote_average }}
                </li>
            </ul>
        </main>
    </div>
</template>

<script>
import axios from "axios";
import Header from "./Header";

export default {
    name: "Main",
    components: {
        Header,
    },
    data() {
        return {
            filmsArray: [],
        };
    },
    methods: {
        apiCall(text) {
            axios
                .get("https://api.themoviedb.org/3/search/movie", {
                    params: {
                        api_key: "365f3969a4eff7fb7d2818a19293db37",
                        query: text,
                    },
                })
                .then((res) => {
                    this.filmsArray = res.data.results;
                    console.log(res.data.results);
                })
                .catch((err) => console.log(err));
        },
        test() {
            console.log(this.filmsArray);
        },
    },
};
</script>

<style scoped lang="scss"></style>
