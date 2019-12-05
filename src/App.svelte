<script>
  let search = "";
  let lang = "ru";
  let words = [];
  let matching = [];
  let regex = /.*/;
  let wholeword = false;
  let error = null;
  let input = null;

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
  $: fetch_words(lang);
  $: make_regex(search, wholeword);
  $: input && input.setCustomValidity(error || "");
  $: matching = words.filter(w => w.match(regex));
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

<ul title="Words matching {regex}">
  {#each matching as word (word)}
    <li>{word}</li>
  {/each}
</ul>
