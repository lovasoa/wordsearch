<script>
  let search = "";
  let lang = "ru";
  let words = [];
  let matching = [];
  let regex = /.*/;
  let wholeword = false;
  let error = null;
  let input = null;
  let load = null;
  const max_matches = 500;

  async function fetch_words(lang) {
    let r = await fetch(`dicts/${lang}.txt`);
    let txt = await r.text();
    words = txt.split("\n");
  }

  function make_regex(search, wholeword) {
    try {
      regex = new RegExp(wholeword ? `^${search}$` : search);
      error = null;
    } catch (e) {
      error = e;
    }
  }

  function match_words(words, regex) {
    matching.length = 0;
    for (var i = 0; i < words.length && matching.length < max_matches; i++) {
      let word = words[i];
      if (regex.test(word)) matching.push(word);
    }
  }

  $: load = fetch_words(lang);
  $: make_regex(search, wholeword);
  $: input && input.setCustomValidity(error || "");
  $: match_words(words, regex);
</script>

<style>
  form {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  form > * {
    margin: 10px;
  }
  input[type="text"] {
    flex: 1;
  }
  li {
    display: inline-block;
    margin: 10px;
    width: 100px;
    overflow: hidden;
  }
  .error {
    color: rgb(243, 89, 89);
  }
  .info {
    color: grey;
  }
</style>

<h1>Wordsearch</h1>

<form>
  <input
    type="text"
    bind:value={search}
    placeholder="Word search"
    bind:this={input} />
  <select bind:value={lang} title="Which dictionnary to search">
    <option>ru</option>
    <option>en</option>
    <option>fr</option>
  </select>
  <label>
    <input type="checkbox" bind:checked={wholeword} />
    Whole word
  </label>
</form>

{#await load}
  <p class="info">Loading...</p>
{:then loaded}
  <ul title="Words matching {regex}">
    {#each matching as word (word)}
      <li>{word}</li>
    {:else}
      <p class="info">No matching words</p>
    {/each}
  </ul>
{:catch error}
  <p class="error">{error.message}</p>
{/await}
