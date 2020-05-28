# Dom Handler

The DOM handler creates a tree containing all nodes of a page.
The tree may be manipulated using the [domutils](https://github.com/fb55/domutils) or [cheerio](https://github.com/cheeriojs/cheerio) libraries.

## Usage

```javascript
const handler = new DomHandler([ <func> callback(err, dom), ] [ <obj> options ]);
// const parser = new Parser(handler[, options]);
```

Available options are described below.

## Example

```javascript
const { Parser } = require("htmlparser2");
const { DomHandler } = require("domhandler");
const rawHtml =
    "Xyz <script language= javascript>var foo = '<<bar>>';< /  script><!--<!-- Waah! -- -->";
const handler = new DomHandler(function (error, dom) {
    if (error) {
        // Handle error
    } else {
        // Parsing completed, do something
        console.log(dom);
    }
});
const parser = new Parser(handler);
parser.write(rawHtml);
parser.end();
```

Output:

```javascript
[
    {
        data: "Xyz ",
        type: "text",
    },
    {
        type: "script",
        name: "script",
        attribs: {
            language: "javascript",
        },
        children: [
            {
                data: "var foo = '<bar>';<",
                type: "text",
            },
        ],
    },
    {
        data: "<!-- Waah! -- ",
        type: "comment",
    },
];
```

## Option: normalizeWhitespace

Indicates whether the whitespace in text nodes should be normalized (= all whitespace should be replaced with single spaces).
The default value is `false`.

For the following examples, this HTML will be used:

```html
<font> <br />this is the text <font></font></font>
```

### Example: `normalizeWhitespace: true`

```javascript
[
    {
        type: "tag",
        name: "font",
        children: [
            {
                data: " ",
                type: "text",
            },
            {
                type: "tag",
                name: "br",
            },
            {
                data: "this is the text ",
                type: "text",
            },
            {
                type: "tag",
                name: "font",
            },
        ],
    },
];
```

### Example: `normalizeWhitespace: false`

```javascript
[
    {
        type: "tag",
        name: "font",
        children: [
            {
                data: "\n\t",
                type: "text",
            },
            {
                type: "tag",
                name: "br",
            },
            {
                data: "this is the text\n",
                type: "text",
            },
            {
                type: "tag",
                name: "font",
            },
        ],
    },
];
```

## Option: withStartIndices

Indicates whether a `startIndex` property will be added to nodes.
When the parser is used in a non-streaming fashion, `startIndex` is an integer indicating the position of the start of the node in the document.
The default value is `false`.

## Option: withEndIndices

Indicates whether a `endIndex` property will be added to nodes.
When the parser is used in a non-streaming fashion, `endIndex` is an integer indicating the position of the end of the node in the document.
The default value is `false`.

---

