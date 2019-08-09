<template>
    <div>
        <v-layout>
            <v-flex xs4>
                <ScoreboardSongComp v-for="entry in scoreboardLeftHalf"
                                    :key="entry.order"
                                    :entry="entry">
                </ScoreboardSongComp>
            </v-flex>
            <v-flex xs4>
                <ScoreboardSongComp v-for="entry in scoreboardRightHalf"
                                    :key="entry.order"
                                    :entry="entry">
                </ScoreboardSongComp>
            </v-flex>
            <v-flex xs4>
                <CurrentVoteComp v-if="voted > 0" :vote="votes[voted - 1]" :songs="songs" :scoreKey="scoreKey"></CurrentVoteComp>
                <v-btn @click="nextVote" v-if="voted < votes.length" class="nextButton">Next Vote</v-btn>
                <div v-if="voted > 0" class="votingStatus">
                    <di>{{voted}} of {{votes.length}} redditors voting</di>
                    <v-progress-linear :value="voted / votes.length * 100"></v-progress-linear>
                </div>
            </v-flex>
        </v-layout>
    </div>
</template>

<script>
    import ScoreboardSongComp from "./ScoreboardSongComp";
    import CurrentVoteComp from "./CurrentVoteComp";
    export default {
        name: "ScoreboardComp",
        components: {CurrentVoteComp, ScoreboardSongComp},
        data: () => ({
            songs: [
                {order: 1, performer: 'Loreen', song: 'Euphoria', points: 0, country: 'se'},
                {order: 2, performer: 'KEiiNO', song: 'Spirit In The Sky', points: 0, country: 'no'},
                {order: 3, performer: 'KEiiNO', song: 'Spirit In The Sky', points: 0, country: 'de'},
                {order: 4, performer: 'KEiiNO', song: 'Spirit In The Sky', points: 0, country: 'no'},
                {order: 5, performer: 'KEiiNO', song: 'Spirit In The Sky', points: 0, country: 'no'},
                {order: 6, performer: 'KEiiNO', song: 'Spirit In The Sky', points: 0, country: 'no'},
                {order: 7, performer: 'KEiiNO', song: 'Spirit In The Sky', points: 0, country: 'no'},
                {order: 8, performer: 'KEiiNO', song: 'Spirit In The Sky', points: 0, country: 'no'},
                {order: 9, performer: 'KEiiNO', song: 'Spirit In The Sky', points: 0, country: 'no'},
                {order: 10, performer: 'KEiiNO', song: 'Spirit In The Sky', points: 0, country: 'no'},
                {order: 11, performer: 'KEiiNO', song: 'Spirit In The Sky', points: 0, country: 'no'},
                {order: 12, performer: 'KEiiNO', song: 'Spirit In The Sky', points: 0, country: 'no'},
                {order: 13, performer: 'KEiiNO', song: 'Spirit In The Sky', points: 0, country: 'no'},
                {order: 14, performer: 'KEiiNO', song: 'Spirit In The Sky', points: 0, country: 'no'},
                {order: 15, performer: 'KEiiNO', song: 'Spirit In The Sky', points: 0, country: 'no'},
                {order: 16, performer: 'KEiiNO', song: 'Spirit In The Sky', points: 0, country: 'no'},
                {order: 17, performer: 'KEiiNO', song: 'Spirit In The Sky', points: 0, country: 'no'},
                {order: 18, performer: 'KEiiNO', song: 'Spirit In The Sky', points: 0, country: 'no'},
                {order: 19, performer: 'KEiiNO', song: 'Spirit In The Sky', points: 0, country: 'no'},
                {order: 20, performer: 'KEiiNO', song: 'Spirit In The Sky', points: 0, country: 'no'},
                {order: 21, performer: 'KEiiNO', song: 'Spirit In The Sky', points: 0, country: 'no'},
                {order: 22, performer: 'KEiiNO', song: 'Spirit In The Sky', points: 0, country: 'no'},
                {order: 23, performer: 'KEiiNO', song: 'Spirit In The Sky', points: 0, country: 'no'},
                {order: 24, performer: 'KEiiNO', song: 'Spirit In The Sky', points: 0, country: 'no'},
                {order: 25, performer: 'KEiiNO', song: 'Spirit In The Sky', points: 0, country: 'no'},
                {order: 26, performer: 'KEiiNO', song: 'Spirit In The Sky', points: 0, country: 'no'},
                {order: 27, performer: 'KEiiNO', song: 'Spirit In The Sky', points: 0, country: 'no'},
                {order: 28, performer: 'KEiiNO', song: 'Spirit In The Sky', points: 0, country: 'no'},
                {order: 29, performer: 'KEiiNO', song: 'Spirit In The Sky', points: 0, country: 'no'},
                {order: 30, performer: 'KEiiNO', song: 'Spirit In The Sky', points: 0, country: 'no'},
                {order: 31, performer: 'KEiiNO', song: 'Spirit In The Sky', points: 0, country: 'no'},
                {order: 32, performer: 'KEiiNO', song: 'Spirit In The Sky', points: 0, country: 'no'},
            ],
            votes: [
                [24, 27, 3, 2, 1, 9, 32, 18, 15, 12, 'Sollunad'],
                [24, 13, 30, 17, 5, 2, 3, 1, 4, 6, 'Voter 2'],
                [27, 30, 32, 2, 1, 9, 32, 18, 15, 12, 'Voter 3']
            ],
            scoreKey: [12, 10, 8, 7, 6, 5, 4, 3, 2, 1],
            voted: 0
        }),
        computed: {
            scoreboardLeftHalf: function() {
                const songCount = this.songs.length;
                return this.songs.slice(0, songCount / 2);
            },
            scoreboardRightHalf: function() {
                const songCount = this.songs.length;
                return this.songs.slice(songCount / 2);
            }
        },
        methods: {
            orderSongs: function() {
                this.songs.sort(this.compareSongs);
            },
            compareSongs: function(a,b) {
                if (a.points === b.points) {
                    return a.order - b.order;
                } else {
                    return b.points - a.points;
                }
            },
            nextVote: function() {
                if (this.voted < this.votes.length) {
                    const vote = this.votes[this.voted];
                    for (let i = 0; i < 10; i++) {
                        const pickedSong = this.songs.find(s => s.order === vote[i]);
                        pickedSong.points += this.scoreKey[i];
                    }
                    this.orderSongs();
                    this.voted++;
                }
            }
        },
        created: function() {
            this.orderSongs();
        }
    }
</script>

<style scoped>
    .nextButton {
        margin: 10px 10px 10px 0;
    }

    .votingStatus {
        margin: 10px;
        text-align: center;
    }
</style>