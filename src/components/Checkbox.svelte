<script>
  export let label = "";
  export let id = Math.floor(Math.random() * 1000000 + 1);
  export let checked = false;
  export let disabled = false
</script>

<div class="checkbox">
  <input class="checkbox__input" {id} type="checkbox" bind:checked disabled={disabled} /><label
    for={id}>{label}</label
  >
</div>

<style>
  .checkbox {
    display: flex;
    align-items: center;
  }
  .checkbox__input {
    margin: 0 0.3em 0 0;
  }
</style>
