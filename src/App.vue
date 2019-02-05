<template>
    <v-app>
        <v-content>
            <!--display a list of available movies-->
            <movie-list></movie-list>
        </v-content>
    </v-app>
</template>

<script>

    import MovieList from './components/MovieList.vue'

    export default {
        name: 'app',
        components: {
            'movie-list': MovieList
        }
    }
</script>

<style>
</style>
