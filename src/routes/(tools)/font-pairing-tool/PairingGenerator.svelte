<script>
  import { onMount } from 'svelte';
  import FontDisplay from './FontDisplay.svelte';

  let fonts = [];
  let selectedFont1 = '';
  let selectedFont2 = '';
  let customText = 'The quick brown fox jumps over the lazy dog';
  let fontSize = 20; // Changed from 16 to 20 for better visibility
  let fontWeight = 400;

  onMount(async () => {
    const res = await fetch('https://www.googleapis.com/webfonts/v1/webfonts?key=YOUR_API_KEY');
    const data = await res.json();
    fonts = data.items.map(font => font.family);
  });

  function savePairing() {
    const pairings = JSON.parse(localStorage.getItem('pairings') || '[]');
    pairings.push({ font1: selectedFont1, font2: selectedFont2, text: customText, size: fontSize, weight: fontWeight });
    localStorage.setItem('pairings', JSON.stringify(pairings));
  }
</script>

<div class="settings-panel">
  <h2>Customize Font Pairing</h2>
  <label for="font1">Font 1:</label>
  <select bind:value={selectedFont1} id="font1">
    {#each fonts as font}
      <option value={font}>{font}</option>
    {/each}
  </select>

  <label for="font2">Font 2:</label>
  <select bind:value={selectedFont2} id="font2">
    {#each fonts as font}
      <option value={font}>{font}</option>
    {/each}
  </select>

  <label for="customText">Custom Text:</label>
  <textarea bind:value={customText} id="customText"></textarea>

  <label for="fontSize">Font Size:</label>
  <input type="number" bind:value={fontSize} id="fontSize" min="8" max="72" />

  <label for="fontWeight">Font Weight:</label>
  <input type="number" bind:value={fontWeight} id="fontWeight" min="100" max="900" step="100" />

  <button on:click={savePairing}>Save Pairing</button>
</div>

<FontDisplay font1={selectedFont1} font2={selectedFont2} text={customText} size={fontSize} weight={fontWeight} />

<style>
  .settings-panel {
    margin-bottom: 20px;
    padding: 20px;
    border: 1px solid #ddd;
    border-radius: 5px;
    background-color: #f9f9f9;
  }
  h2 {
    margin-bottom: 15px;
    font-size: 1.5em;
    color: #333;
  }
  label {
    display: block;
    margin: 10px 0 5px;
    font-weight: bold;
  }
  select, textarea, input {
    display: block;
    width: 100%;
    margin-bottom: 10px;
    padding: 8px;
    border: 1px solid #ddd;
    border-radius: 5px;
  }
  textarea {
    height: 60px;
    resize: none;
  }
  button {
    display: inline-block;
    padding: 10px 15px;
    background-color: #007bff;
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    font-size: 1em;
  }
  button:hover {
    background-color: #0056b3;
  }
</style>
