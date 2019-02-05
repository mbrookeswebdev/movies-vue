<template>
    <div>
        <div id="movie-list">
            <v-container class="my-2">
                <v-layout align-center justify-center>
                    <v-flex xs6 s6 md6 lg6>
                        <h1 class="mb-3 text-md-center heading black--text">Top List 2018</h1>
                        <h2 class="mb-3 text-md-center heading black--text">Vote now for your favourite movie!</h2>
                        <v-expansion-panel>
                            <!--list all available movie titles-->
                            <v-expansion-panel-content class="green accent-2"
                                                       v-for="movie in movies"
                                                       :key="movie.id">
                                <div slot="header" class="black--text title ml-2">
                                    {{ movie.title }}
                                </div>
                                <v-card>
                                    <v-card-text>
                                        <!--load the Movie component with selected movie details-->
                                        <selected-movie :selMovie="movie"></selected-movie>
                                    </v-card-text>
                                </v-card>
                            </v-expansion-panel-content>
                        </v-expansion-panel>
                    </v-flex>
                </v-layout>
            </v-container>

            <!--pagination, using links provided by Laravel backend-->
            <div class="text-xs-center mb-5">
                <v-pagination
                        circle
                        color="teal"
                        v-model="pagination.currentPage"
                        :length="pagination.last"
                        @input="getMovies()"
                ></v-pagination>
            </div>
        </div>
    </div>
</template>
<script>

    import Movie from './Movie.vue'
    import axios from 'axios'

    export default {
        components: {
            'selected-movie': Movie
        },
        data: function () {
            return {
                movies: [],
                errors: [],
                pagination: {
                    currentPage: 1
                },
            };
        },
        mounted() {
            this.getMovies();
        },
        methods: {
            getMovies() {
                //request movie data
                axios.get('http://phplaravel-228259-738640.cloudwaysapps.com/api/movies?page=' + this.pagination.currentPage)
                    .then(response => {
                        this.pagination.currentPage = response.data.current_page;
                        this.pagination.last = response.data.last_page;
                        this.movies = response.data.data;
                    })
                    .catch(e => {
                        this.errors.push(e)
                    })
            }
        }

    }
</script>

<style>
    h1, h2 {
        font-family: 'Roboto Slab', sans-serif;
    }
</style>
