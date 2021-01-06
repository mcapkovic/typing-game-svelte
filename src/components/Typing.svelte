<script>
    import Game from "./Game.svelte";
    import { DEFAULT_STATE } from "../constants";
    import { paragraphs } from "../text";

    const initialText =
        paragraphs[Math.floor(Math.random() * paragraphs.length)];
    let letters = [];
    let possition = -1;

    initialText.split(" ").forEach((word) => {
        [...word].forEach((letter, index) => {
            letters.push({
                value: letter,
                word,
                position: index,
                state: DEFAULT_STATE,
            });
        });
        letters.push({
            value: " ",
            word: " ",
            state: DEFAULT_STATE,
        });
    });

</script>

<div>
    <Game bind:letters bind:possition />
</div>
