<script>
    import {
        DEFAULT_STATE,
        ERROR_STATE,
        VALID_STATE,
        CURSOR_STATE,
    } from "../constants";

    export let letters = [];
    export let possition = -1;

    let prevValueLength = -1;

    let firstErrorIndex = 1000;

    function doMagic(value) {
        if (prevValueLength === -1 && value.length === 0) return;
        if (letters.length < possition) return;

        let newPossition =
            prevValueLength > value.length ? possition - 1 : possition + 1;

        prevValueLength = value.length;

        if (newPossition < 0) {
            letters[newPossition + 1].state = DEFAULT_STATE;
            possition = newPossition;
            return;
        }

        console.log("newPossition", newPossition);
        console.log("letters", letters[newPossition]);
        console.log("value", value[value.length - 1]);
        // const newLetters = [...letters]

        if (newPossition < possition) {
            letters[newPossition + 1].state = DEFAULT_STATE;
            firstErrorIndex =
                firstErrorIndex === newPossition + 1 ? 1000 : firstErrorIndex;
        } else if (letters[newPossition].word === value) {
            // console.log("word match");
            letters[newPossition].state = VALID_STATE;
            inputValue = "";
            prevValueLength = 0;
            firstErrorIndex = 1000;
        } else if (firstErrorIndex < newPossition) {
            letters[newPossition].state = ERROR_STATE;
        } else if (letters[newPossition].value === value[value.length - 1]) {
            letters[newPossition].state = VALID_STATE;
        } else {
            firstErrorIndex =
                firstErrorIndex < newPossition ? firstErrorIndex : newPossition;
            letters[newPossition].state = ERROR_STATE;
        }
        // letters=newLetters
        possition = newPossition;
    }

    let inputValue = "";
    // $: possition = inputValue.length;
    $: doMagic(inputValue);
</script>

<style>
    .letter--default {
        background-color: transparent;
        color: gray;
    }
    .letter--valid {
        background-color: transparent;
    }
    .letter--error {
        background-color: #920000;
    }
    .letter--cursor {
        background-color: transparent;
        position: relative;
        color: gray;
    }
    .letter--cursor::before {
        content: "";
        background-color: #26ffd7;
        width: 1px;
        height: 100%;
        position: absolute;
        left: 0;
        top: 0;
    }
</style>

<div>
    {#each letters as letter, index}
        <span
            class={`letter letter--${index === possition + 1 ? CURSOR_STATE : letter.state}`}>{letter.value}</span>
    {/each}
    <div><input bind:value={inputValue} /></div>
    {possition}
</div>
