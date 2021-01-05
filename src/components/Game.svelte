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
    .game__text__letter{
        border-left: 1px solid transparent;
    }
    .game__text__letter--default {
        background-color: transparent;
        color: gray;
    }
    .game__text__letter--valid {
        background-color: transparent;
    }
    .game__text__letter--error {
        background-color: #920000;
    }
    .game__text__letter--cursor {
        background-color: transparent;
        position: relative;
        color: gray;
        border-left: 1px solid #26ffd7;
    }

    .game {
        padding: 1rem;
        background-color: #2f2f2f;
    }
    .game__text {
        text-align: left;
        font-size: 20px;
    }
    .game__controls {
        text-align: center;
        margin-top: 1rem;
    }
    .game__controls__input {
        margin: 0;
        outline: none;
        border: none;
        background-color: #3a3939;
        color: white;
        width: 100%;
    }

    .game__controls__button {
        margin: 0;
        border: none;
        background: #3a3939;
        color: #ff3e00;
        width: 100%;
    }
    .game__controls__button:hover {
        background: #353535;
    }
</style>

<div class="game">
    <!-- {possition} -->

    <div class="game__text">
        {#each letters as letter, index}
            <span
                class={`game__text__letter game__text__letter--${index === possition + 1 ? CURSOR_STATE : letter.state}`}>{letter.value}</span>
        {/each}
    </div>

    <div class="game__controls">
        <input class="game__controls__input" bind:value={inputValue} />
        <!-- <button class="game__controls__button">start</button> -->
    </div>
</div>
