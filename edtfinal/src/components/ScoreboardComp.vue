<template>
    <div>
        <v-layout>
            <v-flex xs4>
                <ScoreboardSongComp v-for="entry in scoreboardLeftHalf"
                                    :key="entry.order"
                                    :entry="entry"
                                    :vote="currentVote"
                                    :scoreKey="scoreKey">
                </ScoreboardSongComp>
            </v-flex>
            <v-flex xs4>
                <ScoreboardSongComp v-for="entry in scoreboardRightHalf"
                                    :key="entry.order"
                                    :entry="entry"
                                    :vote="currentVote"
                                    :scoreKey="scoreKey">
                </ScoreboardSongComp>
            </v-flex>
            <v-flex xs4>
                <CurrentVoteComp :vote="currentVote" :songs="songs" :scoreKey="scoreKey" v-on:next="nextVote"></CurrentVoteComp>
                <div class="votingStatus">
                    <div>{{voted}} of {{votes.length}} redditors voting</div>
                    <v-slider
                        v-model="voted"
                        min="0"
                        :max="votes.length">
                    </v-slider>
                </div>
                <div>
                    <v-btn @click="nextVote" color="info" :disabled="voted === votes.length" class="nextButton">Next Vote</v-btn>
                    <v-btn @click="lastVote" color="info" :disabled="voted === 0" class="nextButton">Last Vote</v-btn>
                    <v-btn @click="reset" color="error" :disabled="voted === 0" class="nextButton">Reset</v-btn>
                </div>
            </v-flex>
        </v-layout>
    </div>
</template>

<script>
    import ScoreboardSongComp from "./ScoreboardSongComp";
    import CurrentVoteComp from "./CurrentVoteComp";
    import _songs from '../data/songs';
    import _votes from '../data/votes';


    export default {
        name: "ScoreboardComp",
        components: {CurrentVoteComp, ScoreboardSongComp},
        data: () => ({
            songs: [],
            votes: [],
            scoreKey: [12, 10, 8, 7, 6, 5, 4, 3, 2, 1],
            voted: 0,
        }),
        computed: {
            scoreboardLeftHalf: function() {
                const songCount = this.songs.length;
                return this.songs.slice(0, songCount / 2);
            },
            scoreboardRightHalf: function() {
                const songCount = this.songs.length;
                return this.songs.slice(songCount / 2);
            },
            currentVote: function() {
                if (this.voted === 0) return null;
                else return this.votes[this.voted - 1];
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
            lastVote: function() {
                if (this.voted > 0) {
                    this.voted--;
                }
            },
            nextVote: function() {
                if (this.voted < this.votes.length) {
                    this.voted++;
                }
            },
            reset: function() {
                this.voted = 0;
            },
            setToVote: function(vote) {
                if (vote >= 0 && vote <= this.votes.length) {
                    for (const song of this.songs) {
                        song.points = 0;
                    }
                    for (let v = 0; v < vote; v++) {
                        const vote = this.votes[v];
                        for (let i = 0; i < 10; i++) {
                            const pickedSong = this.songs.find(s => s.order === vote[i]);
                            pickedSong.points += this.scoreKey[i];
                        }
                    }
                    this.orderSongs();
                }
            }
        },
        created: function() {
            this.songs = _songs;
            this.votes = _votes;
            this.orderSongs();
        },
        watch: {
            voted: function() {
                this.setToVote(this.voted);
            }
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