<template>
    <div class="game_wheel" @click="handler">
        <div 
            v-for="item in items" 
            :id="item.color" 
            class="wheel_item"
            :key="item.color + ':key'"
            >
            <audio :src="item.asset"/>
        </div>
    </div>
</template>

<script>
export default {
    name: 'game-wheel',
    data() {
        return {
            items: [
                    {
                        color: "red",
                        asset: require("../assets/sounds_1.mp3")
                    },
                    {
                        color: "green",
                        asset: require("../assets/sounds_2.mp3")
                    },
                    {
                        color: "blue",
                        asset: require("../assets/sounds_3.mp3")
                    },
                    {
                        color: "orange",
                        asset: require("../assets/sounds_4.mp3")
                    }
            ]
        }
    },
    props: {
        disabled: Boolean
    },
    methods: {
        handler(_e) {
            if(_e.target.parentElement.className === "game_wheel") {
                if(this.disabled) {
                    return this.itemToggle(_e.target);
                } else {
                    return this.makeStep(_e.target);
                }
            }
        },
        
        makeStep(target) {
            //Find target index to compare it with level sequence
            let ind = Object.values(document.querySelector('.game_wheel').children).indexOf(target);

            this.itemToggle(target);
            
            return this.$emit("step", ind);                
        },
        
        itemToggle(target) {
            target.style.filter = "brightness(1)";

            //Sound overlapping
            let allAudio = document.querySelectorAll('audio');
            allAudio.forEach(el => {
                el.pause(); 
                el.currentTime = 0;
            });

            target.firstElementChild.play(); 
            setTimeout(() => target.style.filter = "", 300);
        }
    }
}
</script>

<style lang="sass" scoped>
.game_wheel
    width: 350px
    height: 350px
    display: grid
    grid-template-columns: 1fr 1fr
    gap: 0
    background-color: gray
    border: solid 2px white
    border-radius: 50%
    transform: rotate(45deg)
    box-shadow: 0 0 3px 3px gray
    overflow: hidden
    transition: .1s

.wheel_item
    filter: brightness(.5)

#red
    background-color: red

#green
    background-color: green

#blue
    background-color: blue

#orange
    background-color: orange

@media screen and (max-width: 450px)
    .game_wheel
        width: 300px
        height: 300px
</style>
