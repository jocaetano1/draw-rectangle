<script lang="ts" setup>
    const props = defineProps({ imageUrl: String });
    type Rectangle = {
        id: string,
        x: number,
        y: number,
        width: number,
        height: number
    }
    let isDrawing = false;
    let isMouseDown = false;
    let isMouseMove = false;
    let rectangles: Rectangle[] = []

    onMounted(() => {
        const containerElement = document.getElementById("container");
        function startRectangle(event: MouseEvent) {
            const idRectangle = Math.trunc(Math.random() * 1000);
            const rectange: Rectangle = {
                id: idRectangle.toString(),
                x: event.clientX,
                y: event.clientY,
                width: 0,
                height: 0
            }
            isDrawing = true;
            rectangles.push(rectange)
        }

        function sizeRectangle(event: MouseEvent){
            if (!isDrawing) return;
            let endX = event.clientX;
            let endY = event.clientY;
            
            const rectangle = rectangles[rectangles.length - 1]
            rectangle.width = Math.abs(endX - rectangle.x);
            rectangle.height = Math.abs(endY - rectangle.y);

            let x = Math.min(rectangle.x, endX)
            let y = Math.min(rectangle.y, endY)
            let rectangleElement = drawElement(x, y, rectangle)

            if (document.getElementById(rectangle.id)){
                let element = document.getElementById(rectangle.id)
                containerElement!.replaceChild(rectangleElement, element as HTMLElement)
            } else {
                containerElement!.appendChild(rectangleElement)
            }

        }
        
        function stopRectangle(event: MouseEvent){
            isDrawing = false;
            const rectangle = rectangles[rectangles.length - 1]
            let x = Math.min(rectangle.x, event.clientX)
            let y = Math.min(rectangle.y, event.clientY)
            attachIcon(x + rectangle.width, y - 13, rectangle.id)
        }

        function drawElement(positionX: number, positionY: number, rectangle: Rectangle){
            let divElement = document.createElement("div")
            divElement.setAttribute("class", "rectangle")
            divElement.setAttribute("id", rectangle.id)
            divElement.style.position = "absolute";
            divElement.style.left = positionX + "px";
            divElement.style.top = positionY + "px";
            divElement.style.width =  rectangle.width + "px"      
            divElement.style.height = rectangle.height + "px"
            return divElement
        }

        function attachIcon(positionX: number, positionY: number, rectangleId: string) {
            const icon = document.createElement("button")
            icon.setAttribute("class", "icon")
            icon.setAttribute("data-rectangle", rectangleId)
            icon.innerText = "x"
            icon.style.left = positionX + "px"; 
            icon.style.top = positionY + "px"; 
            containerElement?.appendChild(icon);
        }

        function deleteRectangle(event: Event){
            console.log(event, "click")
        }



        containerElement!.addEventListener("mousedown", startRectangle)
        containerElement!.addEventListener("mousemove", sizeRectangle)
        containerElement!.addEventListener("mouseup", stopRectangle)
    })
</script>

<template>
    <div id="container" class="w-[800px] h-auto border border-2 border-green-200 relative">
        <img class="mx-auto" id="image" :src="props.imageUrl" draggable="false">
    </div>
</template>

<style>
    .rectangle {
        position: absolute;
        width: 0;
        height: 0;
        border: 1px solid red;
        pointer-events: none;
    }
    .icon {
        border: 1px solid gray;
        width: 20px;
        height: 20px;
        padding: 2px;
        position: absolute;
        border-radius: 100%;
        text-align: center;
        font-size: 10px;
    }
</style>