<script>
  export let correctWords = 0;
  export let className = ''

  let startTime;
  let wpm = 0;

  $: if (!startTime && correctWords > 0) {
    startTime = new Date().getTime();
  }

  $: if (correctWords > 1) {
    const durationInMinutes = (new Date().getTime() - startTime) / 60000.0;
    wpm = (correctWords / durationInMinutes).toFixed(2);
  }
</script>

<div class={className}>{wpm} wpm</div>
