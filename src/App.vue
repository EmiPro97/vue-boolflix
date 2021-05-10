<template>
    <div id="app">
        <Header @apiCallEmitted="apiCall" />
        <Main :filmsArray="originalAllArray" />
    </div>
</template>

<script>
import Header from "@/components/Header";
import Main from "@/components/Main";
import axios from "axios";

export default {
    name: "App",
    components: {
        Header,
        Main,
    },
    data() {
        return {
            originalFilmsArray: [],
            originalSeriesArray: [],
			originalAllArray: [],
        };
    },
    methods: {
        apiCall(text) {
            if (text !== "") {
                axios
                    .get("https://api.themoviedb.org/3/search/movie", {
                        params: {
                            api_key: "365f3969a4eff7fb7d2818a19293db37",
                            query: text,
                        },
                    })
                    .then((res) => {
                        this.originalFilmsArray = res.data.results;
                    })
                    .catch((err) => console.log(err));

                axios
                    .get("https://api.themoviedb.org/3/search/tv", {
                        params: {
                            api_key: "365f3969a4eff7fb7d2818a19293db37",
                            query: text,
                        },
                    })
                    .then((res) => {
                        this.originalSeriesArray = res.data.results;
                    })
                    .catch((err) => console.log(err));

				this.originalAllArray = [...this.originalFilmsArray, ...this.originalSeriesArray];
            }
        },
    },
};
</script>

<style lang="scss">
@import "./styles/general";
</style>
