<script lang="ts">
    import { onMount } from 'svelte';

    let canvas: HTMLCanvasElement;
    let coord = {x: 0, y: 0}

    async function saveImage(canvas: HTMLCanvasElement) {
        let ctx: CanvasRenderingContext2D = canvas.getContext('2d')

        let image:string = canvas.toDataURL()
        let requestBody = {
            'image_data': image
        }

        let response: Response = await fetch('http://localhost:8000', {
            method: 'POST',
            headers: {
                'content-type': 'application/x-www-form-urlencoded'
            },
            body: JSON.stringify(requestBody)
        })

        console.log('Server response')
        console.log(await response.json())

    }

    onMount(() => {
        let ctx: CanvasRenderingContext2D = canvas.getContext('2d')
        ctx.fillStyle = 'white';
        ctx.fillRect(0,0,canvas.width, canvas.height);

        function draw(event: MouseEvent) {
            ctx.beginPath();
            ctx.lineWidth = 5;
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
            bind:this={canvas}
            width={512}
            height={512}
    ></canvas>

    <button on:click={() => saveImage(canvas)}>Save image</button>
</div>

<style>
    canvas { background: #eee; }
</style>