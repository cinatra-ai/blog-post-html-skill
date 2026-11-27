---
name: blog-post-html
description: Rules for turning the approved markdown of one blog post into HTML for a WordPress page. The words are never changed, added, removed or reordered; each markdown element becomes a plain HTML element that WordPress accepts as post content; the answer is the HTML only. Applies to the step that writes the post's HTML.
license: Apache-2.0
---

# Blog post markdown to HTML

You are given the approved markdown of one blog post. You write the HTML that a WordPress page shows as its content. A person reads your HTML on the publish card and approves it, and it is then sent to the site exactly as you wrote it.

This skill applies to the step that writes the HTML, and only that step is offered it. The step that publishes is given the HTML you wrote and sends it unchanged; it does not convert anything again.

## Rule 1: the words do not change

- Never change, add, remove or reorder a word, a number or a punctuation mark of the post.
- Never correct spelling, never translate, never summarise, never add a sentence of your own.
- Only markdown syntax changes: it becomes HTML elements. A markdown backslash escape (`\*`) becomes the character it escapes (`*`).
- You are given no title and you add none. A `# ` heading in the text is part of the text and becomes `<h1>` like any other heading.

## Rule 2: markdown becomes plain HTML

| Markdown | HTML |
| --- | --- |
| `#` to `######` heading | `<h1>` to `<h6>`, the same level (`##` becomes `<h2>`; no level is shifted) |
| paragraph (lines separated by a blank line) | `<p>` |
| line that ends in two spaces or `\` | `<br>` inside the paragraph |
| `*text*` or `_text_` | `<em>` |
| `**text**` or `__text__` | `<strong>` |
| `~~text~~` | `<del>` |
| `` `code` `` | `<code>` |
| fenced or indented code block | `<pre><code>`, its lines exactly as written; the language name after the fence is not output |
| `[text](address)` and `<address>` | `<a href="address">text</a>`; a `"title"` becomes `title="title"` |
| `![alt](address)` | `<img src="address" alt="alt">`; a `"title"` becomes `title="title"` |
| `-`, `*` or `+` list | `<ul>` with `<li>` |
| `1.` list | `<ol>` with `<li>`; a first number other than 1 becomes `start="n"` |
| nested list | the inner `<ul>` or `<ol>` inside its `<li>` |
| `>` block quote | `<blockquote>` with its paragraphs inside |
| `---`, `***` or `___` on its own line | `<hr>` |
| table with a `---` row | `<table>` with `<thead>`, `<tbody>`, `<tr>`, `<th>` and `<td>` |

- A link and an image keep their address exactly as written.
- Only these addresses become a link or an image: one that starts with `http://`, `https://` or `mailto:` (in any letter case), and one that starts with `./`, `../`, `#`, or a single `/` followed by a letter or a digit. An address that starts with `//`, or that contains a backslash, is not one of these. Every other address is not a link: write the link text as plain text, or the image's alt text as plain text. An address that contains a control character (anything below U+0020, or U+007F) is never a link.
- This is deliberately narrow. A relative address written without a leading `./` (for example `login?next=home`) is written as plain text too: the skill does not try to tell such an address from one with a scheme.
- Lines of one paragraph stay in one `<p>`, joined by a line break in the HTML source.

## Rule 3: nothing else goes into the HTML

- No `<script>`, `<style>`, `<iframe>`, `<form>` or `<object>` element.
- No event attribute (`onclick`, `onload` or any other `on...`), no `style` attribute, no `class` and no `id`.
- No wrapper element around the whole post (no `<html>`, `<body>`, `<article>`, `<div>` or `<section>`).
- No title of your own, no HTML comment and no WordPress block comment (`<!-- wp:... -->`).
- HTML that is written in the markdown is text, not markup: escape it (Rule 4).

## Rule 4: escape text

In text and in code, write `&` as `&amp;`, `<` as `&lt;` and `>` as `&gt;`. In an attribute value, also write `"` as `&quot;`. Write every other character as it is, including non-Latin letters and emoji.

## Rule 5: the answer is the HTML only

- Answer with the HTML and nothing else: no sentence before or after it, no code fence around it, no JSON unless the step asks for a JSON envelope, and then the HTML is the value of the field the step names.
- Put each block element (heading, paragraph, list, block quote, code block, rule, table) on its own line, in the order of the markdown.
- An empty post gives an empty answer.

## Examples

Markdown:

```markdown
## Why we moved

Our old office was *too small* for 12 people & a dog.
Read [the plan](https://example.com/plan "Plan").
```

HTML:

```html
<h2>Why we moved</h2>
<p>Our old office was <em>too small</em> for 12 people &amp; a dog.
Read <a href="https://example.com/plan" title="Plan">the plan</a>.</p>
```

Markdown:

```markdown
> Ship small.

3. Write the test
4. Run `npm test` <now>
   - check the output
```

HTML:

```html
<blockquote>
<p>Ship small.</p>
</blockquote>
<ol start="3">
<li>Write the test</li>
<li>Run <code>npm test</code> &lt;now&gt;
<ul>
<li>check the output</li>
</ul>
</li>
</ol>
```

Markdown:

````markdown
![A red door](https://example.com/door.jpg)

```js
if (a < b) alert("hi");
```

<script>steal()</script>
````

HTML:

```html
<p><img src="https://example.com/door.jpg" alt="A red door"></p>
<pre><code>if (a &lt; b) alert("hi");
</code></pre>
<p>&lt;script&gt;steal()&lt;/script&gt;</p>
```
