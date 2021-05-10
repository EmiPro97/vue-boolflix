<template>
    <div class="search-wrapper">
        <input v-model="inputText" type="text" placeholder="Search..." />
        <button @click="$emit('apiCallEmitted', inputText)">Find</button>
    </div>
</template>

<script>
export default {
    name: "Search",
    data() {
        return {
            inputText: "",
        };
    },
};
</script>

<style></style>
