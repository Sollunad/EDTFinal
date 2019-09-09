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
                <CurrentVoteComp :vote="currentVote" :songs="songs" :scoreKey="scoreKey"
                                 v-on:next="nextVote"></CurrentVoteComp>
                <div class="votingStatus">
                    <div>{{voted}} of {{voteCount}} redditors voting</div>
                    <v-slider
                            v-model="voted"
                            min="0"
                            :max="voteCount">
                    </v-slider>
                </div>
                <div>
                    <v-btn @click="nextVote" color="info" :disabled="voted === voteCount" class="button">Next Vote
                    </v-btn>
                    <v-btn @click="lastVote" color="info" :disabled="voted === 0" class="button">Last Vote</v-btn>
                    <v-btn @click="reset" color="error" :disabled="voted === 0" class="button">Reset</v-btn>
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
            // => see data/songs.js
            songs: [],
            // => see data/votes.js
            votes: [],
            scoreKey: [12, 10, 8, 7, 6, 5, 4, 3, 2, 1],
            voted: 0,
        }),
        computed: {
            songCount: function () {
                return this.songs.length;
            },
            voteCount: function () {
                return this.votes.length;
            },
            scoreboardLeftHalf: function () {
                return this.songs.slice(0, this.songCount / 2);
            },
            scoreboardRightHalf: function () {
                return this.songs.slice(this.songCount / 2);
            },
            currentVote: function () {
                if (this.voted === 0) return null;
                else return this.votes[this.voted - 1];
            }
        },
        methods: {
            nextVote: function () {
                if (this.voted < this.voteCount) {
                    this.voted++;
                }
            },
            lastVote: function () {
                if (this.voted > 0) {
                    this.voted--;
                }
            },
            reset: function () {
                this.voted = 0;
            },
            calculatePoints: function () {
                for (const song of this.songs) {
                    song.points = 0;
                }
                for (let v = 0; v < this.voted; v++) {
                    const vote = this.votes[v];
                    for (let i = 0; i < 10; i++) {
                        const pickedSong = this.songs.find(s => s.order === vote[i]);
                        pickedSong.points += this.scoreKey[i];
                    }
                }
                this.orderSongs();
            },
            orderSongs: function () {
                this.songs.sort(this.compareSongs);
            },
            compareSongs: function (a, b) {
                /*
                    Order songs in descending order by their score.
                    If two songs have the same score, the song with the lower running order will be listed first.
                 */
                if (a.points === b.points) {
                    return a.order - b.order;
                } else {
                    return b.points - a.points;
                }
            },
            shuffleArray: function (array) {
                for (let i = array.length - 1; i > 0; i--) {
                    const j = Math.floor(Math.random() * (i + 1));
                    const temp = array[i];
                    array[i] = array[j];
                    array[j] = temp;
                }
            }
        },
        created: function () {
            this.songs = _songs;
            this.votes = _votes;
            // this.shuffleArray(this.votes);
            this.calculatePoints();
        },
        watch: {
            voted: function () {
                this.calculatePoints();
            }
        }
    }
</script>

<style scoped>
    .button {
        margin: 10px 10px 10px 0;
    }

    .votingStatus {
        margin: 10px;
        text-align: center;
    }
</style>