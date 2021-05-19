<template>
    <header
        :class="{ fixedHeader: isFixed }"
        ref="header"
        class="flex align-center justify-between"
    >
        <!-- Header-left-side -->
        <div class="left-wrapper flex align-center">
            <!-- Logo-img -->
            <a class="logo-link" href="#"
                ><img src="../assets/imges/boolflix-logo.png" alt="boolflix-logo"
            /></a>
            <!-- Main-nav -->
            <nav>
                <ul class="flex">
                    <li
                        v-for="(link, index) in headerNav"
                        :key="index"
                        :class="{ active: link.active }"
                        @click="
                            {
                                activeLink(index),
                                    $emit('updateIsEmpty', currentNavID);
                            }
                        "
                    >
                        <a :href="link.url">{{ link.text }}</a>
                    </li>
                </ul>
            </nav>
        </div>
        <!-- Header-right-side -->
        <div class="right-wrapper flex align-center">
            <!-- Search-component -->
            <Search @apiCallEmitted="tmp" />
            <!-- Gift-item -->
            <div>
                <a href="#"><i class="fas fa-gift"></i></a>
            </div>
            <!-- Bell-item -->
            <div>
                <a href="#"><i class="fas fa-bell"></i></a>
            </div>
            <!-- Profile avatar -->
            <div class="avatar-wrap">
                <img src="../assets/imges/avatar.png" alt="avatar-img" />
            </div>
        </div>
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
        getMovies: Function,
        getTvSeries: Function,
        getNewPopular: Function,
        getMyList: Function,
    },
    data() {
        return {
            headerNav: [
                { text: "Home", url: "#", active: true },
                { text: "Movies", url: "#", active: false },
                { text: "Tv Series", url: "#", active: false },
                { text: "New and Popular", url: "#", active: false },
                { text: "My List", url: "#", active: false },
            ],
            currentNavID: 0,
            isFixed: false,
        };
    },
    mounted() {
        // Update isFixed value on scroll
        window.onscroll = () => {
            let header = this.$refs.header;
            if (window.scrollY > header.offsetTop) {
                this.isFixed = true;
            } else {
                this.isFixed = false;
            }
        };
    },
    methods: {
        // Second emit from search component to App +
        // Interactivity with header nav while searching in the input
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

        // Switch element class active on header nav item click +
        // API call to the respective item clicked
        activeLink(index) {
            this.currentNavID = index;
            this.headerNav.forEach((element) => {
                if (element.active === true) {
                    element.active = false;
                }
            });
            this.headerNav[index].active = true;
            if (this.headerNav[index].text === "Home") {
                this.landingPage();
            } else if (this.headerNav[index].text === "Movies") {
                this.getMovies();
            } else if (this.headerNav[index].text === "Tv Series") {
                this.getTvSeries();
            } else if (this.headerNav[index].text === "New and Popular") {
                this.getNewPopular();
            } else if (this.headerNav[index].text === "My List") {
                this.getMyList();
            }
        },
    },
};
</script>

<style scoped lang="scss">
// Vars
@import "../styles/vars";

header {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    z-index: 10;
    background: transparent;
    padding: 15px 60px;
    font-size: 15px;
    transition: all 0.3s;
    &.fixedHeader {
        background: $main-bg;
    }
    .left-wrapper {
        .logo-link {
            position: relative;
            top: 3px;
            img {
                width: 130px;
            }
        }
        ul li {
            margin-left: 20px;
            font-weight: 600;
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
    .right-wrapper {
        & > div:not(:first-child) {
            margin-left: 40px;
            transition: transform 0.5s;
            &:hover {
                transform: scale(1.05);
            }
            &.avatar-wrap {
                border-radius: 5px;
                overflow: hidden;
                width: 35px;
                height: 35px;
                cursor: pointer;
                img {
                    width: 100%;
                    height: 100%;
                }
            }
        }
        i {
            font-size: 22px;
        }
    }
}
</style>
