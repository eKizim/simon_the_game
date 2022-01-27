<template>
<div id="playground">
    <game-info 
        :numArr="numArr" 
        :usrArr="usrArr"
        :level="level" 
        :gameStatus ="gameStatus"
        :controllerDisabled="controllerDisabled"
        @start-game="startGame"
        @restart-game="restartGame"
    />

    <game-wheel 
        :disabled="wheelDisabled"
        @step="usrStep"
    />
            
    <game-difficulty 
        :numArr="numArr"
        :currentDifficulty="currentDifficulty"
        @choose-difficulty="selectDifficulty"
        />
</div>
</template>


<script>
import GameDifficulty from "./GameDifficulty";
import GameInfo from "./GameInfo";
import GameWheel from "./GameWheel";

export default {
    name: "playground",
    components: {
        GameDifficulty,
        GameInfo,
        GameWheel
    },
    data() {
        return {
            numArr: [],
            usrArr: [],
            level: 1,
            difficulty: {
                Easy: 1500,
                Medium: 1000,
                Hard: 400
            },
            currentDifficulty: "Medium",
            gameStatus: "",
            wheelDisabled: true,
            controllerDisabled: false
        }
    },
    methods: {
        selectDifficulty(dif) {
            this.currentDifficulty = dif;
        },
        
        usrStep(step) {
            if(step !== this.numArr[this.usrArr.length]) { // Game over
                this.restartGame();
                this.wheelDisabled = true;
                return this.gameStatus = "loss";
            }

                this.usrArr.push(step);

            if(this.usrArr.length === this.numArr.length) { // Level passed
                this.usrArr = [];
                this.level = this.level + 1;
                
                this.generateNumArr();

                setTimeout(() => this.levelDemo(), 1000); //Timeout before next level demo

                return this.gameStatus = "win";
            }
        },
        
        generateNumArr() {
            let tempArr = [];
            let length = 2 + this.level;

            while(tempArr.length !== length) {
                let num = Math.floor(Math.random() * 4);
                tempArr.push(num);
            }

            this.numArr = tempArr;
        },
        
        levelDemo() {
            let gameWheel = document.querySelector(".game_wheel");
            gameWheel.style.pointerEvents = "none"; // Disable wheel reaction on user click
            this.gameStatus = "";
            
            this.wheelDisabled = true; // Disable user input comparing
            this.controllerDisabled = true; // Disable buttons

            let wheelItems = document.querySelectorAll('.wheel_item');

            for(let i = 0; i < this.numArr.length; i++) {
                setTimeout(() => {
                    wheelItems[this.numArr[i]].click();
                        if(i === this.numArr.length - 1) {
                            gameWheel.style.pointerEvents = "";
                            this.wheelDisabled = false;
                            this.controllerDisabled = false;
                        }
                }, i * this.difficulty[this.currentDifficulty]); // Time depends on difficulty level
            }
        },
        
        startGame() {
            this.generateNumArr();
            this.levelDemo();
        },

        restartGame() {
            this.level = 1;
            this.numArr = [];
            this.usrArr = [];
        }
    }
}
</script>


<style lang="sass" scoped>
#playground
    width: 100%
    max-width: 1300px
    padding: 50px
    display: flex
    align-items: center
    justify-content: space-between
    gap: 50px
    background-color: #c4c4c4
    box-shadow: 0 0 3px 2px gray

@media screen and (max-width: 991px)
    #playground
        flex-direction: column

</style>
