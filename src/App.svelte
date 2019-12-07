<script>
  import Info from "./Info.svelte";
  let url_params = new URL(document.location).searchParams || {
    get: _ => null
  };
  let search = url_params.get("search") || "";
  let lang = url_params.get("lang") || "ru";
  let wholeword = !!url_params.get("wholeword");
  let words = [];
  let matching = [];
  let regex = /.*/;
  let error = null;
  let input = null;
  let load = null;
  let max_matches = 130;

  function replace_url(search, lang, wholeword) {
    try {
      let params = new URLSearchParams({ lang: lang });
      if (search) params.set("search", search);
      if (wholeword) params.set("wholeword", "on");
      window.history.replaceState(
        null,
        "RegEx word search: " + search,
        "?" + params.toString()
      );
    } catch (e) {
      console.log(e);
    }
  }

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

  function match_words(words, regex, max_matches) {
    matching.length = 0;
    for (var i = 0; i < words.length && matching.length < max_matches; i++) {
      let word = words[i];
      if (regex.test(word)) matching.push(word);
    }
  }

  $: load = fetch_words(lang);
  $: make_regex(search, wholeword);
  $: input && input.setCustomValidity(error || "");
  $: match_words(words, regex, max_matches);
  $: replace_url(search, lang, wholeword);
</script>

<style>
  form {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  form > * {
    margin: 1%;
  }
  input[type="search"] {
    flex: 1;
    min-width: 25px;
    height: 35px;
  }
  select {
    height: 35px;
    min-width: 50px;
    max-width: 15%;
  }
  label {
    font-size: 0.8em;
    max-width: 15%;
  }
  ul {
    padding: 0;
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
  }
  li {
    display: inline-block;
    margin: 10px;
    min-width: 100px;
    overflow: hidden;
  }
  small input {
    width: 44px;
    height: 24px;
    padding: 0;
  }
  .error {
    color: rgb(243, 89, 89);
  }
  .info {
    color: grey;
  }
  #max_matches_config {
    padding-bottom: 55px;
  }
  footer {
    position: fixed;
    bottom: 0;
    left: 0;
    padding: 10px;
    padding-top: 20px;
    background: linear-gradient(0deg, white 40%, transparent);
    width: 100%;
    font-size: 0.7em;
  }
</style>

<h1>RegEx-WordSearch</h1>

<Info />

<form method="GET">
  <input
    type="search"
    name="search"
    bind:value={search}
    placeholder="Word search regex"
    bind:this={input} />
  <select bind:value={lang} name="lang" title="Which dictionnary to search">
    <option>ru</option>
    <option>en</option>
    <option>fr</option>
  </select>
  <label
    title="Whether the pattern should match the whole word or any part of it">
    <input type="checkbox" bind:checked={wholeword} name="wholeword" />
    Whole word
  </label>
</form>

{#await load}
  <p class="info">Loading...</p>
{:then loaded}
  <ul title="Words matching {regex}">
    {#each matching as word (word)}
      <li title={word}>{word}</li>
    {:else}
      <p class="info">No matching words</p>
    {/each}
  </ul>
{:catch error}
  <p class="error">{error.message}</p>
{/await}

<small id="max_matches_config">
  Show at most
  <input type="number" min="1" max="10000" bind:value={max_matches} />
  results.
</small>

<footer>
  <strong>RegEx-WordSearch</strong>
  is a free software released by
  <a href="https://github.com/lovasoa">@lovasoa</a>
  under the
  <a href="https://github.com/lovasoa/wordsearch/blob/master/LICENSE">
    BSD3 license
  </a>
  .
  <a href="https://github.com/lovasoa/wordsearch">View the source code</a>
  .
</footer>
