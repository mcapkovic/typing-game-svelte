<script>
    export let position = -1;
    export let noOfCharacters = 0;
    export let className = ''

    let startTime;
    let cpm = 0;

    $: if (!startTime && position > -1) {
        startTime = new Date().getTime();
    }

    $: if (position > 5) {
        const durationInMinutes = (new Date().getTime() - startTime) / 60000.0;
        cpm = (noOfCharacters / durationInMinutes).toFixed(2);
    }
</script>

<div class={className}> {cpm} cpm</div>
