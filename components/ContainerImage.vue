<script lang="ts" setup>

const canvasElement = ref<HTMLCanvasElement>();
const props = defineProps({ imageUrl: String });
const iconUrl = "https://icons.veryicon.com/png/o/object/material_design/close-64.png"
let drawing = false;
let startX: number, startY: number, endX: number, endY: number;
let rectangles: Rectangle[] = [];

type Rectangle = {
    x: number;
    y: number;
    width: number;
    height: number;
};


onMounted(() => {
    const canvas: HTMLCanvasElement | any = document.getElementById("canvas");
    canvas.style.backgroundImage = props.imageUrl;
    const ctx = canvas.getContext("2d");
    
    function drawRectange(x: number, y: number, width: number, height: number) {
        ctx.beginPath();
        ctx.rect(x, y, width, height);
        ctx.stroke();
        createCloseIcon(x, y, width)
    }

    function createCloseIcon(endX: number, endY: number, width: number) {
        const icon = new Image();
        icon.addEventListener("click", () => {
           console.log()
        });
        const iconSize = 20;
        icon.src = iconUrl;
        ctx.drawImage(icon, endX + width - 5, endY - 15, iconSize, iconSize);
    }

    canvasElement.value?.addEventListener("mousedown", (event) => {
        if (canvas) {
            drawing = true;
            startX = event.clientX - canvas.offsetLeft;
            startY = event.clientY - canvas.offsetTop;
            let rectangle: Rectangle = {
                x: startX,
                y: startY,
                width: 0,
                height: 0,
            };
            rectangles.push(rectangle);
        }
    });

    canvasElement.value?.addEventListener("mousemove", (event) => {
        if (!drawing) return;
        if (canvas) {
            const ctx = canvas.getContext("2d");
            endX = event.clientX - canvas.offsetLeft;
            endY = event.clientY - canvas.offsetTop;
            const lastRectangle = rectangles[rectangles.length - 1];
            lastRectangle.width = endX - lastRectangle.x;
            lastRectangle.height = endY - lastRectangle.y;
            ctx.clearRect(0, 0, canvas.width, canvas.height);

            rectangles.forEach((rectangle: Rectangle) => {
                drawRectange(
                    rectangle.x,
                    rectangle.y,
                    rectangle.width,
                    rectangle.height
                );
            });
        }
    });

    canvasElement.value?.addEventListener("mouseup", (event) => {
        drawing = false;
    });
});
</script>

<template>
    <div class="flex justify-center items-center h-screen">
        <canvas
            ref="canvasElement"
            id="canvas"
            class="border border-solid border-4 rounded bg-contain bg-no-repeat border-green-500"
            width="800"
            height="600"
            style="background-image: url(https://enotas.com.br/blog/wp-content/uploads/2020/04/modelo-invoice.png);"
        >
        </canvas>
    </div>
</template>
<style>
</style>
