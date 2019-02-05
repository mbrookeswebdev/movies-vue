<template>
    <div class="movie font-weight-medium subheading">
        <!--list selected movie details-->
        <div class="ml-3 mb-3 font-weight-regular">{{selMovie.descr}}</div>
        <div class="ml-3 body-1">Rating: {{selMovie.rating}}</div>
        <div class="ml-3 body-1">IMDB score: {{selMovie.imdb_score}}</div>
        <div class="ml-3 body-1">No of votes: {{selMovie.noOfVotes}}</div>
        <v-btn dark small v-if="!isHidden" @click="vote()">Vote</v-btn>
        <div class="ml-3 mt-3 body-1" id="message" v-show="voted">{{this.message}}</div>
    </div>
</template>

<script>

    import axios from 'axios'

    export default {
        props: ['selMovie'],
        data: function () {
            return {
                serverResponse: [],
                errors: [],
                message: '',
                voted: false,
                isHidden: false
            }
        },
        methods: {
            vote() {
                // update number of votes and send an update request to Movies API
                this.selMovie.noOfVotes = this.selMovie.noOfVotes + 1;
                let patchValue = {'noOfVotes': this.selMovie.noOfVotes};

                axios.patch('http://phplaravel-228259-738640.cloudwaysapps.com/api/movies/'
                    + this.selMovie.id, patchValue
                )
                    .then(response => {

                        if (response.request.status === 200) {
                            this.message = "Thank you, your vote has been recorded!";
                            this.isHidden = true;
                            this.voted = true;
                        }
                        else {
                            this.message = "Sorry, but your vote couldn't be recorded."
                        }
                    })
                    .catch(e => {
                        this.errors.push(e)
                    })
            }
        }
    }
</script>

<style>
</style>
