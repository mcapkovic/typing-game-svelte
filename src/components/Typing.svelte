<script>
  import Game from "./Game.svelte";
  import WPM from "./WPM.svelte";
  import CPM from "./CPM.svelte";
  import Checkbox from "./Checkbox.svelte";

  import { DEFAULT_STATE } from "../constants";
  import { paragraphs } from "../text";

  function getCharacters(words) {
    const characters = [];
    words.forEach((word, wordIndex) => {
      // get character data
      [...word].forEach((letter, index) => {
        characters.push({
          value: letter,
          word,
          position: index,
          state: DEFAULT_STATE,
        });
      });

      // add space between words
      if (wordIndex < words.length - 1)
        characters.push({
          value: " ",
          word: " ",
          state: DEFAULT_STATE,
        });
    });
    return characters;
  }

  const initialText = paragraphs[Math.floor(Math.random() * paragraphs.length)];
  let characters = [];
  let possition = -1;
  let correctWords = 0;
  let forceLowerCase = false;
  let words;

  function generateContent() {
    characters = getCharacters(words);
  }

  $: if (forceLowerCase) {
    words = [];
    words = initialText.toLowerCase().trim().split(" ");
    generateContent();
  } else {
    words = [];
    words = initialText.trim().split(" ");
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
  <Game bind:letters={characters} bind:possition bind:correctWords />
  <div class="typing__stats">
    <WPM className="typing__stats__tile" {correctWords} />
    <div class="typing__stats__spacer" />
    <CPM
      className="typing__stats__tile"
      noOfCharacters={characters.length}
      position={possition}
    />
  </div>
</div>

<style>
  /* .typing{
    
} */

  .typing__settings {
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
