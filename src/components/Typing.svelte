<script>
  import Game from "./Game.svelte";
  import WPM from "./WPM.svelte";
  import CPM from "./CPM.svelte";

  import { DEFAULT_STATE } from "../constants";
  import { paragraphs } from "../text";

  const initialText = paragraphs[Math.floor(Math.random() * paragraphs.length)];
  let letters = [];
  let possition = -1;
  let correctWords = 0;

  const words = initialText.split(" ");
  words.forEach((word) => {
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

<div class="typing">
  <Game bind:letters bind:possition bind:correctWords />
  <div class="typing__stats">
    <WPM className="typing__stats__tile" {correctWords} />
    <!-- <div class='typing__stats__spacer' /> -->
    <CPM
      className="typing__stats__tile"
      noOfCharacters={letters.length}
      position={possition}
    />
  </div>
</div>

<style>
  /* .typing{
    
} */
  .typing__stats {
    display: flex;
    justify-content: space-between;

  }

  :global(.typing__stats__tile) {
    color: red;
    /* flex-grow: 1; */
    padding: 1em 0;
    margin: 0.5em 0;
    background-color: #2f2f2f;
    color: gray;
    width: 49%;
  }

  /* .typing__stats__spacer{
      width: 0.5em;
  } */
</style>
