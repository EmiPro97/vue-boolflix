<template>
    <div
        class="search-wrapper flex align-center justify-center"
        :class="{ open: searchOpened }"
    >
        <span @click="openSearch" class="search-btn"></span>
        <input
            v-model.trim="inputText"
            @keyup="$emit('apiCallEmitted', inputText)"
            :class="{ open: searchOpened }"
            ref="inputSearch"
            tabindex="0"
            type="text"
            placeholder="Search..."
        />
        <span
            class="close-search"
            :class="{ open: searchOpened }"
            @click="closeSearch"
        ></span>
    </div>
</template>

<script>
export default {
    name: "Search",
    data() {
        return {
            inputText: "",
            searchOpened: false,
        };
    },
    methods: {
        openSearch() {
            this.searchOpened = true;
            this.$refs.inputSearch.focus();
        },
        closeSearch() {
            this.inputText = "";
            this.searchOpened = false;
        },
    },
};
</script>

<style scoped lang="scss">
// Vars
@import "../styles/vars";

.search-wrapper {
    width: 30px;
    border: 1px solid transparent;
    transition: all 0.3s linear;
    &.open {
        border: 1px solid #fff;
        padding: 5px 15px 5px 5px;
        width: 250px;
    }
    .search-btn::before {
        font-family: "Font Awesome 5 Free";
        font-weight: 900;
        content: "\f002";
        font-size: 20px;
        cursor: pointer;
    }
    input {
        border: none;
        outline: none;
        background: none;
        color: $secondary-text;
        width: 0;
        &.open {
            margin-left: 7px;
            flex-grow: 1;
        }
    }
    .close-search {
        &::before {
            font-family: "Font Awesome 5 Free";
            font-weight: 900;
            content: "\f00d";
            font-size: 20px;
            cursor: pointer;
            display: none;
        }
        &.open::before {
            display: block;
            margin-left: 15px;
        }
    }
}
</style>
