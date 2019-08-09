<template>
    <div>
        <div class="vote elevation-10">
            <span class="title">{{voteAuthor}}</span>
        </div>
        <div class="vote elevation-10">
            <div class="title" v-for="(singleVote, index) in votes">
                <v-layout>
                    <v-flex xs2>
                        <span class="points">{{scoreKey[index]}}</span>
                    </v-flex>
                    <v-flex xs10>
                        <span class="song">{{renderSingleVote(singleVote)}}</span>
                    </v-flex>
                </v-layout>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: "CurrentVoteComp",
        props: ['vote', 'songs', 'scoreKey'],
        computed: {
            voteAuthor: function() {
                return this.vote[this.vote.length - 1];
            },
            votes: function() {
                return this.vote.slice(0, this.vote.length - 1);
            }
        },
        methods: {
            renderSingleVote: function(vote) {
                const song = this.songs.find(s => s.order === vote);
                return `${song.song}`;
            }
        }
    }
</script>

<style scoped>
    .vote {
        background: grey;
        margin: 20px 20%;
        padding: 5px;
        height: fit-content;
        width: 60%;
        vertical-align: middle;
        text-align: center;
    }
</style>