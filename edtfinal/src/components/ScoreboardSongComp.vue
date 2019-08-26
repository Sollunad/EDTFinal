<template>
    <div :class="styleObject">
        <FlagIconComp :country="entry.country" class="flag"></FlagIconComp>
        <span class="songInformation subtitle-1">{{entry.performer}} - {{entry.song}}</span>
        <span class="points title">{{entry.points}}</span>
        <span v-if="lastScore" class="points title">+{{lastScore}}</span>
    </div>
</template>

<script>
    import FlagIconComp from "./FlagIconComp";
    export default {
        name: "ScoreboardSongComp",
        components: {FlagIconComp},
        props: ['entry', 'vote', 'scoreKey'],
        computed: {
            isVoted: function() {
                return this.vote && this.vote.includes(this.entry.order);
            },
            lastScore: function() {
                if (this.vote) {
                    const voteIndex = this.vote.indexOf(this.entry.order);
                    if (voteIndex > -1) {
                        return this.scoreKey[voteIndex];
                    }
                }
                return null;
            },
            styleObject: function() {
                return {
                    song: true,
                    'elevation-10': true,
                    isVoted: this.isVoted
                }
            }
        }
    }
</script>

<style scoped>
    .song {
        background: grey;
        margin: 7px 10px;
        padding: 5px;
        height: fit-content;
        vertical-align: middle;
    }

    .isVoted {
        background: darkslateblue;
    }

    .flag {
        margin-right: 7px;
        margin-bottom: -10px;
    }
    .points {
        float: right;
        margin-left: 15px;
    }
</style>