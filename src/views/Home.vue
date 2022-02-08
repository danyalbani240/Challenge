<template>
    <v-stage
        ref="stage"
        @click="handleStageClick"
        @mousemove="handleStageMove"
        :config="configKonva"
    >
        <v-layer>
            <v-rect
                :key="rect.id"
                v-for="rect in rectangles"
                :config="rect"
            ></v-rect>
        </v-layer>
    </v-stage>
</template>

<script>
// @ is an alias to /src
const width = window.innerWidth;
const height = window.innerHeight;
export default {
    name: "Home",
    data() {
        return {
            clickCounter: 0,
            configKonva: {
                width: width,
                height: height,
            },
            rectangles: [
                {
                    x: -1300,
                    y: 940,
                    width: 100,
                    height: 100,
                    fill: "black",
                    stroke: "red",
                    strokeWidth: 1,
                },
            ],
        };
    },
    methods: {
        setRect(obj) {
            this.rectangles.push(obj);
        },
        handleStageClick() {
            const mousePosition = this.$refs.stage
                .getNode()
                .getPointerPosition();
            if (this.clickCounter == 0) {
                console.log(mousePosition);
                ++this.clickCounter;
                const newRect = {
                    x: mousePosition.x,
                    y: mousePosition.y,
                    width: 0,
                    height: 0,
                    stroke: "blue",
                    strokeWidth: 1,
                };
                this.setRect(newRect);
            }
        },
        handleStageMove() {
            if (this.clickCounter == 1) {
                const mousePosition = this.$refs.stage
                    .getNode()
                    .getPointerPosition();

                this.rectangles.at(-1).width =
                    mousePosition.x - this.rectangles.at(-1).x;
                this.rectangles.at(-1).height =
                    mousePosition.y - this.rectangles.at(-1).y;
                console.log(this.rectangles.at(-1));
            }
        },
    },
};
</script>
