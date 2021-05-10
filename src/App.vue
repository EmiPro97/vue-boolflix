<template>
    <div id="app">
        <!-- <Header /> -->
        <Main />
        <div>
            <ul>
                <li></li>
            </ul>
        </div>
    </div>
</template>

<script>
// import Header from "@/components/Header";
import Main from "@/components/Main";

export default {
    name: "App",
    components: {
        // Header,
        Main,
    },
};
</script>

<style lang="scss">
@import "./styles/general";
</style>
