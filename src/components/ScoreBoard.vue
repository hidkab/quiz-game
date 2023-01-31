
<template>
    <div class="score">
        Player <span>{{ this.winCount }}</span> x <span>{{ this.loseCount }}</span> Computer
    </div>
</template>

<script>
    export default {
        props: ['winCount', 'loseCount']
    }
</script>

<style lang="scss">
    div.score {
        border-bottom: 1px solid black;
        padding: 30px;
        font-size: 24px;

        span {
            border: 1px solid black;
            padding: 8px;
            font-weight: bold;
        }
    }

</style>