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
    export default {
        name: "ScoreboardComp",
        components: {CurrentVoteComp, ScoreboardSongComp},
        data: () => ({
            songs: [
                {order: 1, performer: 'Francesco Gabbani', song: 'Occidentali\'s Karma', points: 0, country: 'it'},
                {order: 2, performer: 'maNga', song: 'We Could Be The Same', points: 0, country: 'tr'},
                {order: 3, performer: 'Aminata', song: 'Love Injected', points: 0, country: 'lv'},
                {order: 4, performer: 'Lena', song: 'Satellite', points: 0, country: 'de'},
                {order: 5, performer: 'Kristian Kostov', song: 'Beautiful Mess', points: 0, country: 'bg'},
                {order: 6, performer: 'Sunstroke Project', song: 'Hey Mamma', points: 0, country: 'md'},
                {order: 7, performer: 'Jamala', song: '1944', points: 0, country: 'ua'},
                {order: 8, performer: 'Conchita Wurst', song: 'Rise Like a Phoenix', points: 0, country: 'at'},
                {order: 9, performer: 'KEiiNO', song: 'Spirit In The Sky', points: 0, country: 'no'},
                {order: 10, performer: 'Dami Im', song: 'Sound Of Silence', points: 0, country: 'au'},
                {order: 11, performer: 'Kate Miller-Heidke', song: 'Zero Gravity', points: 0, country: 'au'},
                {order: 12, performer: 'The Common Linnets', song: 'Calm After The Storm', points: 0, country: 'nl'},
                {order: 13, performer: 'Hatari', song: 'Hatrið mun sigra', points: 0, country: 'is'},
                {order: 14, performer: 'Lena', song: 'Taken By A Stranger', points: 0, country: 'de'},
                {order: 15, performer: 'Emmelie de Forest', song: 'Only Teardrops', points: 0, country: 'dk'},
                {order: 16, performer: 'Eugent Bushpepa', song: 'Mall', points: 0, country: 'al'},
                {order: 17, performer: 'Paula Selling & Ovi', song: 'Playing With Fire', points: 0, country: 'ro'},
                {order: 18, performer: 'Loreen', song: 'Euphoria', points: 0, country: 'se'},
                {order: 19, performer: 'Sebalter', song: 'Hunter Of Stars', points: 0, country: 'ch'},
                {order: 20, performer: 'Loïc Nottet', song: 'Rhythm Inside', points: 0, country: 'be'},
                {order: 21, performer: 'Amir', song: 'J\'ai cherché', points: 0, country: 'fr'},
                {order: 22, performer: 'Blanche', song: 'City Lights', points: 0, country: 'be'},
                {order: 23, performer: 'AWS', song: 'Viszlát Nyár', points: 0, country: 'hu'},
                {order: 24, performer: 'Mahmood', song: 'Soldi', points: 0, country: 'it'},
                {order: 25, performer: 'Elina Born & Stig Rästa', song: 'Goodbye To Yesterday', points: 0, country: 'ee'},
                {order: 26, performer: 'Måns Zelmerlöw', song: 'Heroes', points: 0, country: 'se'},
                {order: 27, performer: 'Mørland & Debrah Scarlett', song: 'A Monster Like Me', points: 0, country: 'no'},
                {order: 28, performer: 'Il Volo', song: 'Grande Amore', points: 0, country: 'it'},
                {order: 29, performer: 'Poli Genova', song: 'If Love Was A Crime', points: 0, country: 'bg'},
                {order: 30, performer: 'Eleni Foureira', song: 'Fuego', points: 0, country: 'cy'},
                {order: 31, performer: 'Ermal Meta e Fabrizio Moro', song: 'Non Mi Avete Fatto Niente', points: 0, country: 'it'},
                {order: 32, performer: 'Duncan Laurence', song: 'Arcade', points: 0, country: 'nl'},
            ],
            votes: [
                [24, 27, 3, 2, 1, 9, 32, 18, 15, 12, 'Sollunad'],
                [24, 13, 30, 17, 5, 2, 3, 1, 4, 6, 'Voter 2'],
                [27, 30, 32, 2, 1, 9, 32, 18, 15, 12, 'Voter 3'],
                [32, 25, 30, 28, 31, 9, 2, 18, 15, 12, 'Voter 4']
            ],
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