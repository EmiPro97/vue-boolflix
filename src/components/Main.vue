<template>
    <main class="flex justify-center align-center">
        <div v-if="loading">Loading...</div>
        <div v-else v-show="!isEmpty">
            <div class="container">
                <section v-show="filmsArray.length">
                    <h2 class="responsive-title">Results:</h2>
                    <div class="flex flex-wrap justify-center">
                        <Card
                            v-for="film in filmsArray"
                            :key="film.id"
                            :film="film"
                        />
                    </div>
                </section>
                <section v-show="topWeek.length">
                    <h2 class="responsive-title">Top of the Week:</h2>
                    <div class="flex flex-wrap">
                        <Card v-for="film in topWeek" :key="film.id" :film="film" />
                    </div>
                </section>
            </div>
        </div>
        <div class="no-results" v-show="isEmpty">
            No match found for your research: "{{ inputText }}"
            <p>Some tips:</p>
            <ul>
                <li>Try with other key words</li>
                <li>Use the title of a film or tv series</li>
            </ul>
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
        topWeek: Array,
        loading: Boolean,
        inputText: String,
        isEmpty: Boolean,
    },
};
</script>

<style scoped lang="scss">
// Vars
@import "../styles/vars";

main {
    min-height: 100vh;
    section h2 {
        color: $secondary-text;
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
