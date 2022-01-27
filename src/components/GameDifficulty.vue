<template>
    <div id="game_difficulty">
        <p>Choose game difficulty:</p>
        <ul class="game_difficulty__container" @change="chooseDifficulty">
            <li v-for="item in difficulty" :key="item">
                <input
                    type="radio"
                    name="difficulty"
                    :value="item"
                    :id="item"
                    :checked="item === currentDifficulty"
                    :disabled="numArr.length > 0"
                >
                <label :for="item">{{ item }}</label>
            </li>
        </ul>
    </div>
</template>

<script>
export default {
    name: "game-difficulty",
    data() {
        return {
            difficulty: ["Easy", "Medium", "Hard"]
        }
    },
    props: {
        numArr: Array,
        currentDifficulty: String
    },
    methods: {
        chooseDifficulty(_e) {
            this.$emit("choose-difficulty", _e.target.value);
        }
    }
}
</script>

<style lang="sass" scoped>
#game_difficulty
    display: flex
    flex-direction: column
    align-items: center
    gap: 15px

    p
        font-size: 16px
        font-weight: 900
        text-decoration: underline

.game_difficulty__container
    width: 200px
    display: flex
    flex-direction: column
    gap: 10px
    list-style-type: none

    input
        display: none

    input:checked + label
        color: white
        box-shadow: inset 0 0 2px 1px black

    input:disabled + label
        opacity: .5

    #Easy:checked + label
        background-color: #00c3ff
    #Medium:checked + label
        background-color: #e77a39
    #Hard:checked + label
        background-color: #ee644e

    label
        height: 50px
        display: flex
        align-items: center
        justify-content: center
        gap: 15px
        border-radius: 10px
        box-shadow: 0 0 2px 1px black
</style>
