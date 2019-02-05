<template>
    <div id="animated-glasses">
        <img id="glasses" src="../assets/final.png" />
        <Lights id="lights-square"
            :lightContainer="{x: lightSquareWidth, y: lightSquareHeight}"
            :style="{
                height: lightSquareHeight + 'px',
                top: lightSquareTop + 'px',
             }"
        />
    </div>
</template>

<script>
    import Lights from "./Lights.vue"
    export default {
        name: "animated-glasses",

        components: {
            Lights,
        },

        data() {
            return {
                rendered: false,
            }
        },

        props: {
            windowWidth: Number,
            windowHeight: Number,
        },

        methods: {

        },

        mounted() {
            this.rendered = true;
        },

        computed: {
            lightSquareWidth: function() {
                return 0.5 * this.windowWidth;
            },

            lightSquareHeight: function() {
                return 0.355 * this.lightSquareWidth;
            },
            
            lightSquareTop: function() {
                return 0.95 * this.lightSquareHeight;
            },

        }
    }
</script>

<style scoped>
    #animated-glasses {
        width: 100%;
    }

    #lights-square {
        position: absolute;
        z-index: 1;
        left: 25%;
        width: 50%;
    }

    #glasses {
        z-index: 0;
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: auto;
    }

</style>