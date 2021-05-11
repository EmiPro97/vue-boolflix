<template>
    <main class="flex justify-center align-center">
        <div v-if="loading">Loading...</div>
        <div v-else class="flex flex-wrap justify-center">
            <Card v-for="film in filmsArray" :key="film.id" :film="film" />
        </div>
        <div class="no-results" v-if="isEmpty">
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
        loading: Boolean,
        inputText: String,
        isEmpty: Boolean,
    },
};
</script>

<style scoped lang="scss">
main {
    min-height: 100vh;
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
