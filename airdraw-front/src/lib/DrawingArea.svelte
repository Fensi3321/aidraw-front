<script lang="ts">
    import { onMount } from 'svelte';
    import {predictionResultStore} from "../stores/predictionResultStore";
    import "../../public/app.css"

    let canvas: HTMLCanvasElement;
    let coord = {x: 0, y: 0}

    async function saveImage(canvas: HTMLCanvasElement) {
        let ctx: CanvasRenderingContext2D = canvas.getContext('2d')

        let image: string = canvas.toDataURL()
        let formData: FormData = new FormData()
        formData.append('image_data', image)

        let response: Response = await fetch('http://localhost:8000/predict/canvas', {
            method: 'POST',
            body: formData
        })

        let predictionResult = await response.json()

        predictionResultStore.set(predictionResult)
    }

    onMount(() => {
        let ctx: CanvasRenderingContext2D = canvas.getContext('2d')
        ctx.fillStyle = 'white';
        ctx.fillRect(0,0,canvas.width, canvas.height);

        function draw(event: MouseEvent) {
            ctx.beginPath();
            ctx.lineWidth = 28;
            ctx.lineCap = 'round';
            ctx.strokeStyle = 'black';
            ctx.moveTo(coord.x, coord.y);
            move(event);
            ctx.lineTo(coord.x, coord.y);
            ctx.stroke();
        }

        function start(event: MouseEvent) {
            canvas.addEventListener("mousemove", draw)
            move(event)
        }

        function move(event: MouseEvent) {
            coord.x = event.clientX - canvas.offsetLeft
            coord.y = event.clientY - canvas.offsetTop;
        }

        function stop() {
            canvas.removeEventListener("mousemove", draw)
        }


        function loop() {
            canvas.addEventListener("mousedown", start)
            canvas.addEventListener("mouseup", stop)

            requestAnimationFrame(loop);
        }

        loop();
    });

</script>

<svelte:window />

<div>

    <canvas
            class='mb-5'
            bind:this={canvas}
            width={560}
            height={560}
    ></canvas>

        <div class="content-center">
            <button
            on:click={() => saveImage(canvas)} 
            class="
            bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded-full">
            Predict
        </button>
        </div>
    </div>

<style>
    canvas { background: #eee; }
</style>