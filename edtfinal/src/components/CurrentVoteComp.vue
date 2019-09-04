<template>
    <div>
        <div class="vote elevation-10">
            <div class="title" v-for="i in 10">
                <v-layout>
                    <v-flex xs2 class="points">
                        <span>{{scoreKey[i - 1]}}</span>
                    </v-flex>
                    <v-flex xs10 class="song">
                        <span>{{renderSingleVote(i)}}</span>
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
        methods: {
            renderSingleVote: function(i) {
                if (this.vote) {
                    const song = this.songs.find(s => s.order === this.vote[i - 1]);
                    return `${song.song}`;
                } else {
                    return '';
                }
            }
        }
    }
</script>

<style scoped>
    .vote {
        background: grey;
        margin: 20px 20%;
        height: fit-content;
        width: 60%;
        vertical-align: middle;
        text-align: center;
    }

    .points {
        background: darkslateblue;
    }
</style>