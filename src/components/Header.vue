<template>
    <header>
        <div class="title-wrapper">
            <h1>BOOLFLIX</h1>
        </div>
        <Search @apiCallEmitted="tmp" />
    </header>
</template>

<script>
import Search from "./Search";

export default {
    name: "Header",
    components: {
        Search,
    },
    methods: {
        tmp(inputText) {
            {
                this.$emit("apiCallEmitted", inputText);
            }
        },
    },
};
</script>

<style scoped lang="scss">
// Vars
@import "../styles/vars";

header {
    background: $main-bg;
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 15px 10px;
    h1 {
        color: red;
    }
}
</style>
