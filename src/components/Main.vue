<template>
    <main>
        <div v-if="loading">Loading...</div>
        <div v-else>
            <Card v-for="film in filmsArray" :key="film.id" :film="film" />
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
    },
};
</script>

<style scoped lang="scss"></style>
