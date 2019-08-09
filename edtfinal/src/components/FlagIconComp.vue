<template>
    <img :src="iconLink">
</template>

<script>
    export default {
        name: "FlagIconComp",
        props: ['country'],
        computed: {
            iconLink: function() {
                return `https://www.countryflags.io/${this.country}/flat/32.png`;
            }
        }
    }
</script>

<style scoped>

</style>