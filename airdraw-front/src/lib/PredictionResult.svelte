<script lang="ts">
    import {predictionResultStore} from "../stores/predictionResultStore";
    import {onDestroy} from "svelte";

    let prediction;
    predictionResultStore.subscribe(value  => {
        prediction = value;

        console.log(prediction)
    })

</script>

<div>
    {#each Object.entries(prediction) as [number, prediction]}
        <p>{number}</p>
        <span>{prediction}</span>
    {/each}
</div>