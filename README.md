# regex-wordsearch

## Seach dictionary words using [regular expressions](https://en.wikipedia.org/wiki/Regular_expression)

A small web app to search dictionary words by entering a regex.

https://regex-wordsearch.netlify.com/

## Technical details

The application is written in JavaScript with the [svelte](https://svelte.dev/) framework.

It has a very simple structure:
 - the dictionaries are [simple text files](./public/dicts),
that are [`fetch`ed](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) dynamically.
 - on each key stroke, the regular expression is recompiled, and the dictionary is iterated upon until enough matching words are found.