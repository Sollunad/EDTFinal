<template>
    <div class="song elevation-10">
        <FlagIconComp :country="entry.country" class="flag"></FlagIconComp>
        <span class="songInformation title">{{entry.performer}} - {{entry.song}}</span>
        <span class="points title">{{entry.points}}</span>
    </div>
</template>

<script>
    import FlagIconComp from "./FlagIconComp";
    export default {
        name: "ScoreboardSongComp",
        components: {FlagIconComp},
        props: ['entry'],
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

    .flag {
        margin-right: 7px;
        margin-bottom: -10px;
    }

    .songInformation {

    }

    .points {
        float: right;
    }
</style>