<template>
    <div id="app">
        <Header @apiCallEmitted="apiCall" />
        <Main
            :filmsArray="originalAllArray"
            :loading="loadingStatus"
            :inputText="inputText"
            :isEmpty="isEmpty"
        />
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
            originalAllArray: [],
            loadingStatus: false,
            inputText: "",
            isEmpty: false,
        };
    },
    created() {
        this.landingPageCall();
    },
    methods: {
        apiCall(text) {
            this.isEmpty = false;
            this.loadingStatus = true;
            this.inputText = text;
            if (text !== "") {
                axios
                    .all([
                        axios.get("https://api.themoviedb.org/3/search/movie", {
                            params: {
                                api_key: "365f3969a4eff7fb7d2818a19293db37",
                                query: text,
                            },
                        }),
                        axios.get("https://api.themoviedb.org/3/search/tv", {
                            params: {
                                api_key: "365f3969a4eff7fb7d2818a19293db37",
                                query: text,
                            },
                        }),
                    ])
                    .then(
                        axios.spread((...res) => {
                            this.originalAllArray = [
                                ...res[0].data.results,
                                ...res[1].data.results,
                            ];
                            let tmp = this.originalAllArray;
                            tmp.forEach((el, index) => {
                                if (el.poster_path == null && el.overview == "") {
                                    tmp.splice(index, 1);
                                }
                            });
                            this.loadingStatus = false;
                            if (this.originalAllArray.length === 0) {
                                this.isEmpty = true;
                            }
                        })
                    )
                    .catch((err) => console.log(err));
            }
        },

        landingPageCall() {
            this.loadingStatus = true;
            axios
                .get(
                    "https://api.themoviedb.org/3/trending/all/week?api_key=365f3969a4eff7fb7d2818a19293db37"
                )
                .then((res) => {
                    this.originalAllArray = res.data.results;
                    this.loadingStatus = false;
                })
                .catch((err) => console.log(err));
        },
    },
};
</script>

<style lang="scss">
@import "./styles/general";
@import "./styles/utilities";
</style>
