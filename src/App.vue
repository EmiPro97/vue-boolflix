<template>
    <div id="app">
        <Header @apiCallEmitted="apiCall" />
        <Main :filmsArray="originalAllArray" :loading="loadingStatus" />
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
            loadingStatus: false,
        };
    },
    methods: {
        apiCall(text) {
            this.loadingStatus = true;
            // setTimeout(() => {
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
                        .catch((err) => console.log(err))
                        .finally(() => {
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
                                .catch((err) => console.log(err))
                                .finally(() => {
                                    this.originalAllArray = [...this.originalFilmsArray, ...this.originalSeriesArray];
                                    this.loadingStatus = false;
                                });
                        });
                }
            // },3000);

           

				
        },
    },
};
</script>

<style lang="scss">
@import "./styles/general";
</style>
