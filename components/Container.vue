<script lang="ts" setup>

    const props = defineProps({ imageUrl: String });
    type Rectangle = {
        x: number,
        y: number,
        width: number,
        height: number
    }
    let isDrawing = false;
    let rectangles: Rectangle[] = []

    onMounted(() => {
        const containerElement = document.getElementById("container");
        const imageElement = document.getElementById("image");
        let startX: any, startY: any, endX: any, endY: any;
        
        function startRectangle(event: MouseEvent) {
            const rectange: Rectangle = {
                x: event.clientX,
                y: event.clientY,
                width: 0,
                height: 0
            }
            isDrawing = true;
            rectangles.push(rectange)
            console.log(rectangles)
        }

        function drawRectangle (event: MouseEvent){
            if (!isDrawing) return;
            event.preventDefault()
            let endX = event.clientX;
            let endY = event.clientY;

            const currentRectangle = rectangles[rectangles.length - 1]
            let width = Math.abs(endX - currentRectangle.x);
            let height = Math.abs(endY - currentRectangle.y);
            let areaElement = document.createElement("div")
            areaElement.setAttribute("class", "rectangle")
            areaElement.style.position = "absolute";
            areaElement.style.left = Math.min(currentRectangle.x, endX) + "px";
            areaElement.style.top = Math.min(currentRectangle.y, endY) + "px";"px";
            areaElement.style.width =  width + "px"      
            areaElement.style.height = height + "px"

            if (!containerElement?.hasChildNodes()){
                containerElement?.appendChild(areaElement)
                console.log()
            }
            containerElement?.replaceChild(areaElement, areaElement)
            console.log(event)
            
        }
        
        function endRectangle(event: MouseEvent){
            isDrawing = false;
            const currentRectangle = rectangles[rectangles.length - 1]
            let width = Math.abs(endX - currentRectangle.x);
            let height = Math.abs(endY - currentRectangle.y);
            let areaElement = document.createElement("div")
            areaElement.setAttribute("class", "rectangle")
            areaElement.style.position = "absolute";
            areaElement.style.left = Math.min(currentRectangle.x, endX) + "px";
            areaElement.style.top = Math.min(currentRectangle.y, endY) + "px";"px";
            areaElement.style.width =  width + "px"      
            areaElement.style.height = height + "px"
            containerElement?.appendChild(areaElement)
        }

        containerElement!.addEventListener("mousedown", startRectangle)
        containerElement!.addEventListener("mousemove", drawRectangle)
        containerElement!.addEventListener("mouseup", endRectangle)

    })
</script>

<template>
    <div id="container" class="w-[800px] y-[600px] border border-2 border-green-200 relative">
        <img id="image" :src="props.imageUrl" draggable="false">
    </div>
</template>

<style>
    .rectangle {
        position: absolute;
        width: 0;
        height: 0;
        border: 1px solid red;
        background-color: red;
        pointer-events: none;
    }
</style>