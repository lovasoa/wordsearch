<style>
  section {
    background: radial-gradient(#f4f4f4, #ddd);
    border-radius: 20px;
    height: 15px;
    width: 15px;
    padding: 10px;
    font-size: 15px;
    box-shadow: 0px 0px 3px #555;
    position: fixed;
    top: 10px;
    right: 10px;
    transition: 0.7s;
    font-size: 12px;
    overflow: hidden;
  }
  h2 {
    width: 100%;
    margin: 0;
    text-align: center;
    display: block;
    white-space: nowrap;
    font-size: 1.3em;
  }
  #question {
    max-width: 0;
    height: 1em;
    overflow: hidden;
    display: inline-block;
    color: transparent;
    transition: 0.7s;
  }
  #help {
    width: 500px;
    opacity: 0;
    transition: 0s 0.7s width, 0.1s 0s opacity;
  }
  section:hover {
    width: 500px;
    max-width: 85%;
    height: 600px;
    max-height: 90%;
  }
  section:hover #question {
    max-width: 500px;
    color: black;
  }
  section:hover #help {
    width: 100%;
    opacity: 1;
  }
  table {
    margin-top: 5px;
    width: 100%;
    border-collapse: collapse;
    border-spacing: 0px;
  }
  table,
  th,
  td {
    padding: 3px;
    border: 1px solid black;
  }
  code {
    background: #ddd;
    border: 1px solid #888;
    border-radius: 2px;
    padding: 2px;
    margin: 1px;
    display: inline-block;
  }
</style>

<section>
  <h2 title="More info...">
    <span id="question">How to use this tool&nbsp;</span>?
  </h2>
  <div id="help">
    <h3>Regular expressions</h3>
    <p>
    <a href="https://en.wikipedia.org/wiki/Regular_expression">Regular expressions</a>
    allow you to define a search pattern. The words in the
    chosen dictionary are then filtered, and only those matching the pattern are
    displayed.
    </p><p>
    If you want to learn regular expressions in depth, you can start with
    <a href="https://www.regular-expressions.info/quickstart.html">this guide on regular-expressions.info</a>.
    </p>
    <h3>Examples</h3>
    <p>
    The first column shows a pattern, the second an example of a word it
    matches, the third an example of a word it doesn't match.
    Hover over an example for more information.
    </p>
    <table>
      <thead>
        <tr>
          <th>RegEx</th>
          <th>matches</th>
          <th>doesn't match</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td>
            <code title="'b', followed by any character, followed by 'g'">
              b.g
            </code>
          </td>
          <td>
            <code>bag</code>
            <code>big</code>
            <code>bug</code>
          </td>
          <td>
            <code title="misses a character between b and g">bg</code>
            <code title="more than one character between b and g">bang</code>
          </td>
        </tr>
        <tr>
          <td>
            <code
              title="either a 'w' or nothing, followed by 'ord'">
              w?ord
            </code>
          </td>
          <td>
            <code>word</code>
            <code>ord</code>
          </td>
          <td>
            <code title="misses a 'd'">worm</code>
          </td>
        </tr>
        <tr>
          <td>
            <code
              title="'p', followed by any number of 'o', followed by 't'">
              po*t
            </code>
          </td>
          <td>
            <code>pt</code>
            <code>pot</code>
            <code>poot</code>
          </td>
          <td>
            <code title="misses an 'o'">put</code>
            <code title="has an extra 'r'">port</code>
          </td>
        </tr>
        <tr>
          <td>
            <code
              title="'b', followed by any character any number of time, followed by 'g'">
              b.*g
            </code>
          </td>
          <td>
            <code>bg</code>
            <code>bag</code>
            <code>bang</code>
          </td>
          <td>
            <code title="misses a 'b'">hang</code>
          </td>
        </tr>
        <tr>
          <td>
            <code
              title="'p' or 'r', followed by 'ant'">
              [pr]ant
            </code>
          </td>
          <td>
            <code>pant</code>
            <code>rant</code>
          </td>
          <td>
            <code title="misses a 'p' or an 'r' at the beginning">ant</code>
          </td>
        </tr>
        <tr>
          <td>
            <code
              title="'b' or 'g', followed by any two characters, followed by 'g'">
              [bg]..g
            </code>
          </td>
          <td>
            <code>bang</code>
            <code>gang</code>
          </td>
          <td>
            <code title="has a single character in the middle">big</code>
            <code title="has no character in the middle">hg</code>
          </td>
        </tr>
        <tr>
          <td>
            <code
              title="neither a 'b' nor an 'f' nor a 'p', followed by 'oot'">
              [^bfp]oot
            </code>
          </td>
          <td>
            <code>loot</code>
          </td>
          <td>
            <code title="the first letter is a 'b'">boot</code>
            <code title="the first letter is an 'f'">foot</code>
            <code title="the first letter is missing">oot</code>
          </td>
        </tr>
        <tr>
          <td>
            <code
              title="either 'hu' or 'po', followed by any two characters">
              (hu|po)..
            </code>
          </td>
          <td>
            <code>hurt</code>
            <code>poll</code>
          </td>
          <td>
            <code title="does not start with 'hu' or 'po'">host</code>
            <code title="has less then two characters after 'hu'">hue</code>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</section>
