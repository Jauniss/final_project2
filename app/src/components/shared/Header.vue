<template>
    <header id="menu">
        <nav class="container">
            <ul>
                <li id="drop">
                    <p>À propos</p>
                    <ul class="dropped">
                        <li class="mb-20">
                            <a @click="scrollInto('#how-it-work')">Comment ça marche ?</a>
                        </li>
                        <li class="mb-10">
                            <a @click="scrollInto('#who-we-are')">Qui sommes-nous ?</a>
                        </li>
                    </ul>
                </li>
                <li class="ml-40">
                    <router-link to="/order">Commander</router-link>
                </li>
                <li class="ml-40">
                    <router-link to="/contact">Contact</router-link>
                </li>
            </ul>
            <router-link to="/" class="logo ml-60">
                <img src="../../assets/logo2.png" alt="">
            </router-link>
            <ul class="ml-60">
                <template v-if="!user">
                    <li>
                        <router-link to="/sign-in">Connexion</router-link>
                    </li>
                    <li class="ml-40 button">
                        <router-link to="/sign-up">Inscription</router-link>
                    </li>
                </template>
                <template v-if="user">
                    <li>
                        <router-link to="/account">Mon compte</router-link>
                    </li>
                    <li class="ml-40 button" @click.prevent="disconnect">
                        <router-link to="javascript:void(0)">Déconnection</router-link>
                    </li>
                </template>
                <li class="ml-40" id="basket">
                    <router-link to="/cart">
                        <img src="../../assets/basket.png" alt="">
                        <span class="after">{{quantity}}</span>
                    </router-link>
                </li>
            </ul>
        </nav>
    </header>
</template>

<script>
    import {mapGetters, mapActions} from 'vuex'

    export default {
        name: "Header",
        props: {
            user: Boolean,
            quantity: 0
        },
        data() {
            return {
                drop: false,
                nb_items: 0,
                quantity: 0
            }
        },
        watch: {
            getQuantity: function (val) {
                this.quantity = val
            }
        },
        methods: {
            ...mapActions([
                'newQuantity'
            ]),
            scrollInto(ancre) {
                let road = window.location.hash.split('#')
                if (road[1] != '/') {
                    if (ancre != '#ship')
                        this.$router.push(`/${ancre}`)
                    else
                        this.$router.push(`/`)
                } else {
                    document.querySelector(ancre).scrollIntoView({
                        behavior: 'smooth'
                    });

                }
            },
            disconnect() {
                this.$cookies.remove('user')
                this.$emit('disconnect')
                this.$router.push('/')
            }
        },
        mounted() {
            let cart = this.$cookies.get('cart')
            if (cart != null) {
                cart = JSON.parse(cart)
                this.newQuantity(cart.cart)
            }
            this.quantity = this.getQuantity
            console.log(this.quantity)
        },
        computed: {
            ...mapGetters([
                'getQuantity'
            ])
        }
    }
</script>

<style scoped lang="scss">
    #menu {
        align-self: flex-start;
        position: fixed;
        top: 0;
        display: flex;
        flex-direction: row;
        align-items: center;
        height: 10vh;
        width: 100%;
        box-shadow: 0px 3px 6px rgba(99, 150, 189, .16);
        background-color: #FBFBFB;
        z-index: 1000;
        nav {
            display: flex;
            flex-direction: row;
            align-items: center;
            justify-content: center;
            .logo img {
                cursor: pointer;
                margin-top: 10px;
                width: 145px;
            }
            ul {
                height: 100%;
                display: flex;
                align-items: center;
                justify-content: space-between;
                li {
                    cursor: pointer;
                }
                a {
                    text-decoration: none;
                    color: black;
                }

                &:last-child {
                    li:last-child {
                        a {
                            img {
                                width: 30px;
                            }
                            position: relative;
                            .after {
                                font-size: 10px;
                                position: absolute;
                                width: 15px;
                                height: 15px;
                                background-color: #E45353;
                                color: white;
                                bottom: 0;
                                right: 0;
                                border-radius: 50%;
                                display: flex;
                                align-items: center;
                                justify-content: center;
                            }
                        }
                    }
                }
            }
            #drop:hover {
                text-align: right;
                padding-top: 28.25%;
                position: relative;
                p {
                    margin-bottom: 83px;
                }
                .dropped {
                    display: flex;
                }
            }
            .dropped {
                display: none;
                position: absolute;
                text-align: right;
                -webkit-box-orient: vertical;
                -webkit-box-direction: normal;
                -ms-flex-direction: column;
                flex-direction: column;
                -webkit-box-pack: end;
                -ms-flex-pack: end;
                justify-content: flex-end;
                align-items: flex-end;
                height: auto;
                width: 200px;
                background-color: #FBFBFB;
                padding: 15px;
                text-align: right;
                box-shadow: 0px 5px 6px rgba(106, 146, 183, .2);
                bottom: -10%;
                right: 0;
                a {
                    &:after {
                        display: none !important;
                    }
                }
            }
        }
    }
</style>
