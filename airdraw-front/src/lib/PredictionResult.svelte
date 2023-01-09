<script lang="ts">
    import {predictionResultStore} from "../stores/predictionResultStore";
    import {onDestroy} from "svelte";

    let prediction;
    predictionResultStore.subscribe(value  => {
        let temp = {}

        Object.entries(value).forEach((k, _) => {
            let num = (Number(k[1]) * 100)
            let roundedStr = num.toFixed(2)
            
            temp[k[0]] = roundedStr
        })
        
        console.log(temp)

        prediction = temp
    })

</script>

<div class="ml-5 text-left">

    {#if Object.keys(prediction).length > 0}
        <h3 class="text-3xl font-bold">Here's what I think...</h3>
    {/if}

    {#each Object.entries(prediction) as [number, prediction]}
        <p class="my-2">I'm sure this is <span class="font-bold">{number}</span> with confidence of <span class="font-bold">{prediction}</span> percent</p>
    {/each}
</div>