<template>
    <v-stage
        ref="stage"
        @click="handleStageClick"
        @mousemove="handleStageMove"
        :config="configKonva"
    >
        <v-layer>
            <v-line
                :key="index"
                stroke="blue"
                :strokeWidth="1"
                :points="line"
                v-for="(line, index) in lines"
            ></v-line>

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
            //counter for clicks that count times of clicks
            clickCounter: 0,

            configKonva: {
                width: width,
                height: height,
            },
            //listing rectangles and lines
            rectangles: [],
            lines: [],
        };
    },
    methods: {
        //setting the rectangles and lines with pushing them
        setRect(obj) {
            this.rectangles.push(obj);
        },
        setLine(obj) {
            this.lines.push(obj);
        },
        handleStageClick() {
            const mousePosition = this.$refs.stage
                .getNode()
                .getPointerPosition();
            if (this.clickCounter == 0) {
                ++this.clickCounter;
                //making the first vue of rectangle the next stage will be in line 89
                const newRect = {
                    x: mousePosition.x,
                    y: mousePosition.y,
                    width: 0,
                    height: 0,
                    stroke: "blue",
                    strokeWidth: 1,
                };
                this.setRect(newRect);
            } else if (this.clickCounter == 1) {
                ++this.clickCounter;
                //creating new rect as the same size uf the previous rect /line 97
                const newRect = { ...this.rectangles.at(-1) };
                newRect.x =
                    this.rectangles.at(-1).x + this.rectangles.at(-1).width;

                newRect.y =
                    this.rectangles.at(-1).y + this.rectangles.at(-1).height;

                this.setRect(newRect);
            } else if (this.clickCounter == 2) {
                //here we  drop the last rect and set the lines to make it a cube
                this.addLines();
                this.clickCounter = 0;
            }
        },
        handleStageMove() {
            const mousePosition = this.$refs.stage
                .getNode()
                .getPointerPosition();
            if (this.clickCounter == 1) {
                //making the rectangle size change by mouse move, on the next click it will be fully setted / next stage will be next click
                this.rectangles.at(-1).width =
                    mousePosition.x - this.rectangles.at(-1).x;
                this.rectangles.at(-1).height =
                    mousePosition.y - this.rectangles.at(-1).y;
            } else if (this.clickCounter == 2) {
                //by mouse move it moves / by clicking we will drop it / line  81
                this.rectangles.at(-1).x = mousePosition.x;
                this.rectangles.at(-1).y = mousePosition.y;
            }
        },
        addLines() {
            const firstRect = this.rectangles.at(-2);
            const lastRect = this.rectangles.at(-1);
            const topLeft = [firstRect.x, firstRect.y, lastRect.x, lastRect.y];
            const topRight = [
                firstRect.x + firstRect.width,
                firstRect.y,
                lastRect.x + lastRect.width,
                lastRect.y,
            ];
            const BottomLeft = [
                firstRect.x,
                firstRect.y + firstRect.height,
                lastRect.x,
                lastRect.y + lastRect.height,
            ];
            const BottomRight = [
                firstRect.x + firstRect.width,
                firstRect.y + firstRect.height,
                lastRect.x + lastRect.width,
                lastRect.y + lastRect.height,
            ];
            this.setLine(topLeft);
            this.setLine(topRight);
            this.setLine(BottomLeft);
            this.setLine(BottomRight);
        },
    },
};
</script>
