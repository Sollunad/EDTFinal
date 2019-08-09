<template>
    <div class="vote elevation-10">
        <span class="title">{{voteAuthor}}</span>
    </div>
</template>

<script>
    export default {
        name: "CurrentVoteComp",
        props: ['vote', 'songs'],
        computed: {
            voteAuthor: function() {
                return this.vote[this.vote.length - 1];
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