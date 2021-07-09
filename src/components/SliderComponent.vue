<template>
    <div>
        <div v-if="loading">Loading...</div>
        <VueSlickCarousel v-bind="settings" v-if="games.length > 0">
            <div v-for="game in games" :key="game.GameId">
                <img :src="game.IconUrl" alt="">
            </div>
        </VueSlickCarousel>
    </div>
</template>

<script>
    import VueSlickCarousel from 'vue-slick-carousel'
    import 'vue-slick-carousel/dist/vue-slick-carousel.css'
    import axios from 'axios'

    export default {
        name: "SliderComponent",
        components: { VueSlickCarousel },
        data(){
            return{
                loading: false,
                games: {},
                settings: {
                    "dots": false,
                    "infinite": false,
                    "speed": 500,
                    "slidesToShow": 4,
                    "slidesToScroll": 4,
                    "initialSlide": 0,
                    "responsive": [
                        {
                            "breakpoint": 1024,
                            "settings": {
                                "slidesToShow": 3,
                                "slidesToScroll": 3,
                                "infinite": true,
                                "dots": true
                            }
                        },
                        {
                            "breakpoint": 600,
                            "settings": {
                                "slidesToShow": 2,
                                "slidesToScroll": 2,
                                "initialSlide": 2
                            }
                        },
                        {
                            "breakpoint": 480,
                            "settings": {
                                "slidesToShow": 1,
                                "slidesToScroll": 1
                            }
                        }
                    ]
                }
            }
        },
        created() {
            this.getGames()
        },
        methods:{
            async getGames(){
                this.loading = true
                let self = this
                await axios({
                    url: 'http://api-games.parbatgamessolutions.com/api/Games?s=1',
                    method: "get"
                }).then(response => {
                    self.loading = false
                    self.games = response.data.Games
                }).catch(err => {
                    self.loading = false
                    console.log(err)
                })
            }
        }
    }
</script>

<style scoped>

</style>