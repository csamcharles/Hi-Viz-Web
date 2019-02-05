<template>
    <div id="lights">
        <div id="top-row">
            <SingleLight
                    class="singleLight"
                    v-for="(light, index) in lightsArray"
                    v-bind:key="index"
                    v-bind:style="{
                        left: light.xpos + 'px',
                        top: light.ypos + 'px',
                        height: light.height + 'px',
                        width: light.width + 'px',
                        }"
            />
        </div>
        <div id="bottom-row">
            <SingleLight
                    class="singleLight"
                    v-for="(light, index) in lightsArray"
                    v-bind:key="index"
                    v-bind:style="{
                        left: light.xpos + 'px',
                        top: -light.ypos + 'px',
                        height: light.height + 'px',
                        width: light.width + 'px'
                        }"
            />
        </div>
    </div>
</template>

<script>
    import SingleLight from "./SingleLight.vue";

    export default {
        name: "lights",
        props: {
            lightContainer: {x: Number, y: Number},
        },

        components: {
            SingleLight,
        },

        data: function () {
            return {
                ready: false,
                numLights: 10,
                lensAngleX: 90,
                lensAngleY: 5,
            }
        },

        mounted() {
            this.ready = true;
        },

        methods: {

            toRadians: function (angle) {
                return angle * (Math.PI / 180);
            },

            calculateIncrementalAngle: function (inputAngle) {
                return inputAngle / (this.numLights - 1);
            },

           
        },


        computed: {
             lightsArray: function () {
                let curAngleX = 45;
                let lightsArray = [];
                const lightHeight = this.lightContainer.y / 5;
                const incrementalAngleX = this.calculateIncrementalAngle(this.lensAngleX);
                const stretchNormalization = Math.cos(this.toRadians(curAngleX));
                for (let i = 0; i < this.numLights; i++) {

                    let radians = this.toRadians(curAngleX);
                    curAngleX += incrementalAngleX;
                    const stretchFactor = (Math.cos(radians) * -1);

                    const lightWidth = lightHeight * (1 - Math.abs(stretchFactor));
                    const adjustedLightHeight = lightHeight * (1 - (0.2 * Math.abs(stretchFactor)));

                    let xpos = (this.lightContainer.x / 2) + (stretchFactor / stretchNormalization * this.lightContainer.x / 2) - (lightHeight / 2);
                    let ypos = (stretchFactor * stretchFactor) * 30;
                    const adjustedXpos = xpos + (lightHeight / 2) - (lightWidth / 2);

                    lightsArray[i] = {
                        "xpos": adjustedXpos,
                        "height": adjustedLightHeight,
                        "ypos": ypos,
                        "width": lightWidth,
                    };
                }
                return lightsArray;
            },

        },
    }


</script>

<style scoped>

    #lights {
        width: 100%;
        height: 100%;
    }

    #top-row {
        position: absolute;
        width: 100%;
        top: 0;
    }

    #bottom-row {
        position: absolute;
        width: 100%;
        bottom: 0;
    }

    .singleLight {
        position: absolute;
    }


</style>