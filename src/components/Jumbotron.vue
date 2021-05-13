<template>
    <div
        v-if="dataReady && settedArray.length"
        class="jumbotron flex flex-row-reverse"
    >
        <div
            class="jumbo-img-wrapper"
            :style="{
                backgroundImage:
                    'url(https://image.tmdb.org/t/p/original' +
                    jumboImgArray[randomIndex].backdrop_path +
                    ')',
            }"
        ></div>
        <div class="jumbo-infos">
            <h2>
                {{
                    this.jumboImgArray[randomIndex].title ||
                        this.jumboImgArray[randomIndex].name
                }}
            </h2>
            <p>{{ this.jumboImgArray[randomIndex].overview }}</p>
        </div>
    </div>
</template>

<script>
import axios from "axios";

export default {
    name: "Jumbotron",
    props: {
        settedArray: Array,
    },
    data() {
        return {
            jumboImgArray: [],
            randomIndex: 0,
            dataReady: false,
        };
    },
    async mounted() {
        await axios
            .get(
                "https://api.themoviedb.org/3/trending/all/week?api_key=365f3969a4eff7fb7d2818a19293db37"
            )
            .then((res) => {
                this.jumboImgArray = res.data.results;
                this.randomIndex =
                    Math.floor(Math.random() * (this.jumboImgArray.length - 1)) + 1;
                this.dataReady = true;
            })
            .catch((err) => console.log(err));
    },
};
</script>

<style scoped lang="scss">
.jumbotron {
    height: 580px;
    .jumbo-img-wrapper {
        width: 65%;
        height: 100%;
        background-size: cover;
        background-repeat: no-repeat;
        background-position: center;
        box-shadow: 50px 0 80px 40px #111 inset;
    }
    .jumbo-infos {
        flex-basis: 35%;
        margin-left: 80px;
        margin-top: 100px;
        h2 {
            font-size: 50px;
            margin-bottom: 20px;
        }
    }
}
</style>
