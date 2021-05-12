<template>
    <div id="app">
        <Header
            @apiCallEmitted="apiCall"
            :landingPage="landingPageCall"
            @updateIsEmpty="updateIsEmpty"
        />
        <Main
            :filmsArray="originalAllArray"
            :topWeek="topWeek"
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
            topWeek: [],
        };
    },
    created() {
        this.landingPageCall();
    },
    methods: {
        apiCall(text) {
            if (text === this.inputText) {
                return;
            } else if (text !== "") {
                this.topWeek = [];
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
                            let tmp = this.originalAllArray;
                            tmp.forEach((el, index) => {
                                if (
                                    el.poster_path == null &&
                                    el.overview.trim() == ""
                                ) {
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
            } else {
                this.landingPageCall();
            }
        },

        landingPageCall() {
            if (this.topWeek.length === 0) {
                this.originalAllArray = [];
                this.loadingStatus = true;
                axios
                    .get(
                        "https://api.themoviedb.org/3/trending/all/week?api_key=365f3969a4eff7fb7d2818a19293db37"
                    )
                    .then((res) => {
                        this.topWeek = res.data.results;
                        this.loadingStatus = false;
                    })
                    .catch((err) => console.log(err));
            }
        },

        updateIsEmpty() {
            this.isEmpty = false;
        },
    },
};
</script>

<style lang="scss">
@import "./styles/general";
@import "./styles/utilities";
</style>
