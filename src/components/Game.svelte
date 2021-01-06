<script>
    import {
        DEFAULT_STATE,
        ERROR_STATE,
        VALID_STATE,
        CURSOR_STATE,
        GAME_ACTIVE,
        GAME_INACTIVE,
        GAME_FINISHED,
        GAME_TIMEOUT,
    } from "../constants";
    import Countdown from "./Countdown.svelte";

    export let letters = [];
    export let possition = -1;

    let gameStatus = GAME_INACTIVE;
    let inputValue = "";
    let prevValueLength = -1;
    let firstErrorIndex = Number.MAX_VALUE;

    function textValidation(value) {
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

        if (newPossition < possition) {
            letters[newPossition + 1].state = DEFAULT_STATE;
            firstErrorIndex =
                firstErrorIndex === newPossition + 1 ? Number.MAX_VALUE : firstErrorIndex;
        } else if (letters[newPossition].word === value) {
            inputValue = "";
            prevValueLength = 0;
            firstErrorIndex = Number.MAX_VALUE;

            [...value].forEach((_, index) => {
                letters[newPossition - index].state = VALID_STATE;
            });
        } else if (firstErrorIndex < newPossition) {
            letters[newPossition].state = ERROR_STATE;
        } else if (letters[newPossition].value === value[value.length - 1]) {
            letters[newPossition].state = VALID_STATE;
        } else {
            firstErrorIndex =
                firstErrorIndex < newPossition ? firstErrorIndex : newPossition;
            letters[newPossition].state = ERROR_STATE;
        }
        possition = newPossition;
    }

    $: textValidation(inputValue);

    $: if (possition === letters.length - 1 && firstErrorIndex === Number.MAX_VALUE)
        gameStatus = GAME_FINISHED;
        
    function focusInput(input) {
        input.focus();
    }
</script>

<style>
    .game__text__letter {
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
        animation: 700ms linear infinite alternate cursor-animation;
    }

    @keyframes cursor-animation {
        0% {
            border-left-color: #26ffd7;
        }
        40% {
            border-left-color: #26ffd7;
        }
        60% {
            border-left-color: transparent;
        }
        100% {
            border-left-color: transparent;
        }
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
        color: #e6e6e6;
        width: 100%;
        font-weight: bold;
        cursor: pointer;
    }
    .game__controls__button:hover {
        background: #353535;
        color: white;
    }
</style>

<div class="game">
    <div class="game__text">
        {#each letters as letter, index}
            <span
                class={`game__text__letter game__text__letter--${index === possition + 1 ? CURSOR_STATE : letter.state}`}>{letter.value}</span>
        {/each}
    </div>

    <div class="game__controls">
        {#if gameStatus === GAME_ACTIVE}
            <input
                class="game__controls__input"
                bind:value={inputValue}
                use:focusInput />
        {/if}

        {#if gameStatus === GAME_INACTIVE}
            <button
                class="game__controls__button"
                on:click={() => (gameStatus = GAME_TIMEOUT)}>START</button>
        {/if}

        {#if gameStatus === GAME_TIMEOUT}
            <Countdown bind:gameStatus />
        {/if}
    </div>
</div>
