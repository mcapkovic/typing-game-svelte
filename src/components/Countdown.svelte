<script>
    import { onMount, onDestroy } from "svelte";
    import { GAME_ACTIVE } from "../constants";

    export let gameStatus = "";

    let count = 3;

    let id = 0;

    onMount(() => {
        id = setInterval(function () {
            count--;
        }, 1000);
    });

    onDestroy(() => {
        clearInterval(id);
    });

    $: if (count === 0) gameStatus = GAME_ACTIVE;
</script>

<style>
    .countdown {
        padding: 0.4em;
        background-color: #3a3939;
        font-weight: bold;
            }
</style>

<div class="countdown">{count}</div>
