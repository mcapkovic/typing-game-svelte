<script>
  import Game from "./Game.svelte";
  import WPM from "./WPM.svelte";
  import CPM from "./CPM.svelte";
  import Checkbox from "./Checkbox.svelte";

  import { DEFAULT_STATE } from "../constants";
  import { paragraphs } from "../text";

  const initialText = paragraphs[Math.floor(Math.random() * paragraphs.length)];
  let letters = [];
  let possition = -1;
  let correctWords = 0;
  let forceLowerCase = false;
  let words;

  function generateContent() {
    letters = [];
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
  }

  $: if (forceLowerCase) {
    words = [];
    words = initialText.toLowerCase().split(" ");
    generateContent();
  } else {
    words = [];
    words = initialText.split(" ");
    generateContent();
  }
</script>

<div class="typing">
  <div class="typing__settings">
    <Checkbox
      id="lowerCase"
      label="only lower case"
      bind:checked={forceLowerCase}
      disabled={possition !== -1}
    />
  </div>
  <Game bind:letters bind:possition bind:correctWords />
  <div class="typing__stats">
    <WPM className="typing__stats__tile" {correctWords} />
    <div class="typing__stats__spacer" />
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

.typing__settings{
    margin-bottom: 1em;
}
  .typing__stats {
    display: flex;
  }
  :global(.typing__stats__tile) {
    color: red;
    flex: 1 1 0px;
    padding: 1em 0;
    margin: 0.5em 0;
    background-color: #2f2f2f;
    color: gray;
  }

  .typing__stats__spacer {
    width: 0.5em;
  }
</style>
