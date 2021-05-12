<template>
    <header class="flex align-center justify-between">
        <div class="left-wrapper flex align-center">
            <a class="logo-link" href="#"
                ><img src="../assets/imges/boolflix-logo.png" alt="boolflix-logo"
            /></a>
            <nav>
                <ul class="flex">
                    <li
                        v-for="(link, index) in headerNav"
                        :key="index"
                        :class="{ active: link.active }"
                        @click="
                            {
                                $emit('updateIsEmpty'), activeLink(index);
                            }
                        "
                    >
                        <a :href="link.url">{{ link.text }}</a>
                    </li>
                </ul>
            </nav>
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
    props: {
        landingPage: Function,
    },
    data() {
        return {
            headerNav: [
                { text: "Home", url: "#", active: true },
                { text: "Films", url: "#", active: false },
                { text: "Tv Series", url: "#", active: false },
                { text: "New and Popular", url: "#", active: false },
                { text: "My List", url: "#", active: false },
            ],
            currentNavID: 0,
        };
    },
    methods: {
        tmp(inputText) {
            if (inputText !== "") {
                this.headerNav.forEach((element, index) => {
                    if (element.active === true) {
                        this.currentNavID = index;
                        element.active = false;
                    }
                });
            } else {
                this.headerNav[this.currentNavID].active = true;
            }
            {
                this.$emit("apiCallEmitted", inputText);
            }
        },

        activeLink(index) {
            this.headerNav.forEach((element) => {
                if (element.active === true) {
                    element.active = false;
                }
            });
            this.headerNav[index].active = true;
            if (this.headerNav[index].text === "Home") {
                this.landingPage();
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
    padding: 15px 60px;
    .logo-link img {
        width: 130px;
    }
    ul li {
        margin-left: 20px;
        font-weight: 700;
        color: $header-text;
        transition: color 0.5s;
        &:first-child {
            margin-left: 40px;
        }
        &:hover {
            color: rgba($header-text, 0.7);
        }
        &.active {
            color: $main-text;
            a {
                cursor: default;
            }
        }
    }
}
</style>
