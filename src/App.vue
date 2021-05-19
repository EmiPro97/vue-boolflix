<template>
    <div id="app">
        <Header
            @apiCallEmitted="apiCall"
            :landingPage="landingPageCall"
            :getMovies="getMovies"
            :getTvSeries="getTvSeries"
            :getNewPopular="getNewPopular"
            :getMyList="getMyList"
            @updateIsEmpty="updateIsEmpty"
        />

        <Jumbotron :settedArray="settedArray" />

        <Main
            :filmsArray="originalAllArray"
            :settedArray="settedArray"
            :loading="loadingStatus"
            :inputText="inputText"
            :isEmpty="isEmpty"
            :currentNavID="currentNavID"
        />
    </div>
</template>

<script>
import Header from "@/components/Header";
import Jumbotron from "@/components/Jumbotron";
import Main from "@/components/Main";
import axios from "axios";

export default {
    name: "App",
    components: {
        Header,
        Main,
        Jumbotron,
    },
    data() {
        return {
            originalAllArray: [],
            loadingStatus: false,
            inputText: "",
            isEmpty: false,
            settedArray: [],
            headerNav: [
                { text: "Home", url: "#", active: true },
                { text: "Movies", url: "#", active: false },
                { text: "Tv Series", url: "#", active: false },
                { text: "New and Popular", url: "#", active: false },
                { text: "My List", url: "#", active: false },
            ],
            currentNavID: 0,
        };
    },
    created() {
        // API call on page refresh, return an array
        this.landingPageCall();
    },
    methods: {
        // Main function, API call if something is researched in the input
        apiCall(text) {
            if (text === this.inputText) {
                return;
            } else if (text !== "") {
                this.settedArray = [];
                this.isEmpty = false;
                this.loadingStatus = true;
                this.inputText = text;
                const apiParams = {
                    api_key: "365f3969a4eff7fb7d2818a19293db37",
                    query: text,
                };
                axios
                    .all([
                        axios.get("https://api.themoviedb.org/3/search/movie", {
                            params: apiParams,
                        }),
                        axios.get("https://api.themoviedb.org/3/search/tv", {
                            params: apiParams,
                        }),
                    ])
                    .then(
                        axios.spread((...res) => {
                            this.originalAllArray = [
                                ...res[0].data.results,
                                ...res[1].data.results,
                            ];
                            this.originalAllArray = [...this.originalAllArray].filter(
                                (el) => {
                                    return (
                                        el.poster_path !== null &&
                                        Boolean(el.overview)
                                    );
                                }
                            );
                            this.loadingStatus = false;
                            if (this.originalAllArray.length === 0) {
                                this.isEmpty = true;
                            }
                        })
                    )
                    .catch((err) => console.log(err));
            } else {
                this.isEmpty = false;
                if (this.headerNav[this.currentNavID].text === "Home") {
                    this.landingPageCall();
                } else if (this.headerNav[this.currentNavID].text === "Movies") {
                    this.getMovies();
                } else if (this.headerNav[this.currentNavID].text === "Tv Series") {
                    this.getTvSeries();
                } else if (
                    this.headerNav[this.currentNavID].text === "New and Popular"
                ) {
                    this.getNewPopular();
                } else if (this.headerNav[this.currentNavID].text === "My List") {
                    this.getMyList();
                }
            }
        },

        landingPageCall() {
            this.originalAllArray = [];
            this.loadingStatus = true;
            axios
                .get(
                    "https://api.themoviedb.org/3/trending/all/week?api_key=365f3969a4eff7fb7d2818a19293db37"
                )
                .then((res) => {
                    this.settedArray = res.data.results;
                })
                .catch((err) => console.log(err))
                .finally(() => {
                    this.loadingStatus = false;
                });
        },

        // Return top rated movies
        getMovies() {
            this.originalAllArray = [];
            this.loadingStatus = true;
            axios
                .get(
                    " https://api.themoviedb.org/3/movie/top_rated?api_key=365f3969a4eff7fb7d2818a19293db37"
                )
                .then((res) => {
                    this.settedArray = res.data.results;
                    this.loadingStatus = false;
                })
                .catch((err) => console.log(err));
        },

        // Return top rated tv series
        getTvSeries() {
            this.originalAllArray = [];
            this.loadingStatus = true;
            axios
                .get(
                    "https://api.themoviedb.org/3/tv/top_rated?api_key=365f3969a4eff7fb7d2818a19293db37"
                )
                .then((res) => {
                    this.settedArray = res.data.results;
                    this.loadingStatus = false;
                })
                .catch((err) => console.log(err));
        },

        // Return in theatres movies
        getNewPopular() {
            this.originalAllArray = [];
            this.loadingStatus = true;
            axios
                .get(
                    "https://api.themoviedb.org/3/movie/now_playing?api_key=365f3969a4eff7fb7d2818a19293db37"
                )
                .then((res) => {
                    this.settedArray = res.data.results;
                    this.loadingStatus = false;
                })
                .catch((err) => console.log(err));
        },

        // Return my list
        getMyList() {
            this.originalAllArray = [];
            this.settedArray = [];
        },

        // Update isEmpty value (used in main component) on header-nav click
        updateIsEmpty(index) {
            this.currentNavID = index;
            this.isEmpty = false;
            this.headerNav.forEach((element) => {
                if (element.active === true) {
                    element.active = false;
                }
            });
            this.headerNav[index].active = true;
        },
    },
};
</script>

<style lang="scss">
@import "./styles/general";
@import "./styles/utilities";
</style>
