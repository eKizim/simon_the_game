<template>
    <div id="game_info">
        <p v-if="gameStatus === 'win'" class="round_passed">Level Passed!</p>
        <p v-else-if="gameStatus === 'loss'" class="game_over">Game Over!</p>
        <p v-else class="score">Score: {{ score }}</p>
        <p class="score_reaction">{{reaction}}</p>
        <p class="level">Level: {{level}}</p>
        
        <button 
            class="game_button"
            @click="gameController"
            :disabled="controllerDisabled"
        >
        {{controllerMode}}
        </button>
    </div>
</template>

<script>
export default {
    name: 'game-info',
    props: {
        numArr: Array,
        usrArr: Array,
        level: Number,
        gameStatus: String,
        controllerDisabled: Boolean
    },
    methods: {
        gameController() {
            return this.numArr.length > 0 ? this.$emit("restart-game") : this.$emit("start-game");
        },

        generateNums() {
            this.$emit('generate');
        }
    },
    computed: {
        score() {
            return  (this.level - 1) * 10;
        },
        controllerMode() {
            return this.numArr.length > 0 ? "Restart" : "Start";
        },
        reaction() {
            return (
                this.score >= 500 ? "why?" :
                this.score >= 300 ? "GODLIKE" :
                this.score >= 200 ? "Can't believe" :
                this.score >= 100 ? "Wow!" :
                this.score >= 50 ? "Not bad" : null
            )
        }
    }
}
</script>

<style lang="sass" scoped>
#game_info
    width: 200px
    display: flex
    flex-direction: column
    align-items: center
    gap: 10px

    .score,
    .round_passed,
    .game_over
        font-size: 24px
        font-weight: 900

    .round_passed
        color: green

    .game_over
        color: red

    .score_reaction
        font-style: italic
        font-size: 1.2rem
        font-weight: 900
        text-decoration: underline
        color: #d15a5a

    .level
        font-size: 18px
        text-decoration: underline

    .game_button
        width: 100%
        height: 50px
        border: none
        border-radius: 15px
        font-size: 16px
        box-shadow: 0 0 3px 2px gray
</style>
