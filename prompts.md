
I want to write a web app as a single HTML file using HTML, Javascript and CSS. The app will allow readers to load a citable corpus of Greek texts, and test whether its text contents are consistent with a defined orthographic system. We'll develop it gradually. I want to take advantage of two libraries, the `cex.js` library and the `greeklib.js` library. Please include them with lines like this:

`<script src="https://cdn.jsdelivr.net/gh/neelsmith/cex-lib/cex.js"></script>`

`<script src="https://cdn.jsdelivr.net/gh/neelsmith/greeklib@1.1.0/greeklib.js"></script>`



First I want to build a UI for loading and organizing the data the user will analyze. Begin by letting the user enter a URL for a citable corpus of texts. Use this as the default value:
`https://raw.githubusercontent.com/neelsmith/eagl-texts/refs/heads/main/texts/lysias1.cex`
To load a citable text corpus from a URL, I want to use the `cex.js` library. 
Then we'll use its `loadFromUrl` function with one parameter, the user-selected URL. This will return a `Promise<CEXParser>`. When we have a parser instance, we'll use `getDelimitedData("ctsdata")` to find citable text data (CTS data) in that source. Here's an example from the library documentation of how that could work:
```
const parser = new CEXParser();
parser.loadFromUrl(url)
    .then(p => {
        console.log('CEX data loaded from URL!');
        console.log(p.getDelimitedData("ctsdata"));
    })
    .catch(error => console.error('Failed to load from URL:', error));
  
  
```
`getDelimitedData` will return a string with each line representing a citable passage of text. Please split this into an Array of strings with one entry for each line.

With the text corpus extracted, we can structure it for analysis using the `greeklib.js` library, available here:
`https://cdn.jsdelivr.net/gh/neelsmith/greeklib@1.1.0/greeklib.js`.
We'll first tokenize the corpus, using the array of strings as the only parameter to `greeklib.tokenize`. This will return an Array of `Token` objects. Each `Token` has four properties, `sequence` (an integer), and three string values named `urn`, `text` and `type`, which we'll use later.


Start by displaying the number of tokens in the corpus. Display the `text` andd `urn` properties of the first five tokens in the corpus.

---


Excellent. Next I want to use the function `greeklib.literarygreek()` to create an `Orthography` object. The `Orthography` object has a method named `charset` that returns an Array of string values that are valid in this orthography.

For each of the first five tokens, sucessively test each character in its `text` property. Highlight the display the `text` property so that valid characters are colored green and invalid characters are colored red.

---

Great! Now let's modify the comparison: instead of checking the raw value of the `text` property, first normalize the string to use precomposed forms of Greek.
---

Super. Now compile a list of tokens with invalid forms by testing the normalized precomposed value of the `text` property of all tokens in the corpus. Display the number of invalid tokens you found.

---

Excellent. Now give the user the option of displaying all invalid tokens. If the user chooses to display them, display each token as follows:

- the value of the `urn` property
- the `text` property highlighted so that valid characters are colored green and invalid characters are colored red