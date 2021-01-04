<script>
    import Game from "./Game.svelte";
    import { DEFAULT_STATE } from "../constants";

    const initialText =
        "this industry would become extinct if something were to happen to sever the connection between external expressions and the internal nature of men and women.";
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

    // console.log(letters);
</script>

<div>
    <Game bind:letters={letters}  bind:possition={possition} />
</div>
