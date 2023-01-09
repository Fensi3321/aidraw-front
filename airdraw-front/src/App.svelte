<script>
    import DrawingArea from "./lib/DrawingArea.svelte";
    import PredictionResult from "./lib/PredictionResult.svelte";
    import "../public/app.css"
</script>
<div class="bg-zinc-700">

    <div class="text-center text-white py-10 px-6">
        <h1 class="text-5xl font-bold mt-0">AIdraw</h1>
        <h3 class="text-3xl font-bold">Numbers edition, draw a numer and I'll try to predict it!</h3>

        <div class="flex items-center justify-center h-screen">
            <DrawingArea/>
            <PredictionResult/>
        </div>
    </div>
</div>
