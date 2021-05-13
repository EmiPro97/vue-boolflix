<template>
    <main>
        <div v-if="loading" class="centered-div flex justify-center align-center">
            <div>Loading...</div>
        </div>
        <div v-else v-show="!isEmpty">
            <div class="container">
                <section class="researched-array" v-show="filmsArray.length">
                    <h2 class="responsive-title">Results:</h2>
                    <div class="flex flex-wrap justify-center">
                        <Card
                            v-for="film in filmsArray"
                            :key="film.id"
                            :film="film"
                        />
                    </div>
                </section>
                <section v-show="settedArray.length">
                    <h2 class="responsive-title">
                        {{ this.titleArray[currentNavID] }}
                    </h2>
                    <div class="flex flex-wrap">
                        <Card
                            v-for="film in settedArray"
                            :key="film.id"
                            :film="film"
                        />
                    </div>
                </section>
                <section
                    class="my-list"
                    v-show="
                        currentNavID === 4 &&
                            filmsArray.length === 0 &&
                            settedArray.length === 0
                    "
                >
                    <h2 class="responsive-title">
                        {{ this.titleArray[currentNavID] }}
                    </h2>
                    <div>
                        <h3>There are no elements saved in your list</h3>
                    </div>
                </section>
            </div>
        </div>
        <div v-show="isEmpty" class="centered-div flex justify-center align-center">
            <div class="no-results">
                No match found for your research: "{{ inputText }}"
                <p>Some tips:</p>
                <ul>
                    <li>Try with other key words</li>
                    <li>Use the title of a film or tv series</li>
                </ul>
            </div>
        </div>
    </main>
</template>

<script>
import Card from "./Card";

export default {
    name: "Main",
    components: {
        Card,
    },
    props: {
        filmsArray: Array,
        settedArray: Array,
        loading: Boolean,
        inputText: String,
        isEmpty: Boolean,
        currentNavID: Number,
    },
    data() {
        return {
            titleArray: [
                "Top of the Week:",
                "Most Rated Movies:",
                "Most Rated Tv Series:",
                "In theatres now:",
                "My List:",
            ],
        };
    },
};
</script>

<style scoped lang="scss">
// Vars
@import "../styles/vars";

main {
    .centered-div {
        min-height: 100vh;
    }
    section h2 {
        margin-left: 3px;
        margin-bottom: 20px;
        color: $secondary-text;
    }
    .researched-array,
    .my-list {
        padding-top: 100px;
    }
    .my-list h2 {
        margin-left: 0;
        margin-bottom: 10px;
    }
    .no-results {
        font-size: 18px;
        p {
            margin: 15px 0;
        }
        ul {
            list-style: disc;
            padding-left: 30px;
            li {
                margin-bottom: 5px;
            }
        }
    }
}
</style>
