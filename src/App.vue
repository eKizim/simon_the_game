<template>
    <div id="main_container">
        <playground/>
        <h1 class="main_container__title">Simon Says Game</h1>
        </div>
    </div>
</template>

<script>
import Playground from "./components/Playground";

export default {
    name: "app",
    data() {
        return {}
    },
    components: {
        Playground
    }
}
</script>

<style lang="sass">
*
    margin: 0
    padding: 0
    box-sizing: border-box
    font-family: Ubuntu

#main_container
    padding: 25px
    display: flex
    flex-direction: column
    align-items: center
    gap: 50px

.main_container__title
    padding: 25px
    display: block
    text-align: center
    border: solid 5px black

</style>
