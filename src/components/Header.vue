<template>
    <header>
        <div class="title-wrapper">
            <h1>BOOLFLIX</h1>
        </div>
        <div class="search-wrapper">
            <input v-model="inputText" type="text" placeholder="Search..." />
            <button @click="$emit('apiCallEmitted', inputText)">Find</button>
        </div>
    </header>
</template>

<script>
export default {
    name: "Header",
    data() {
        return {
            inputText: "",
        };
    },
};
</script>

<style scoped lang="scss">
header {
    background: #000;
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 15px 10px;
    h1 {
        color: red;
    }
}
</style>
