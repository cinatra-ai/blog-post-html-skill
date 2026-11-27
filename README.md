# Cinatra Blog Post HTML Skill

The rules a model follows to turn a blog post's approved markdown into HTML for a WordPress page. WordPress stores a post's text as it is given, so markdown sent as is shows headings and links as raw symbols. This skill states the conversion exactly: the words of the post stay as the person approved them, and each markdown element becomes a plain HTML element that WordPress accepts as post content. It is the knowledge half of the HTML step in `@cinatra-ai/blog-wordpress-publish-agent`, packaged as its own skill.

**Install:** Install `@cinatra-ai/blog-post-html-skill` in your Cinatra instance. `@cinatra-ai/blog-wordpress-publish-agent` installs it automatically as a declared dependency.

**Usage:** The skill is delivered into a run by the extension that depends on it. You do not invoke it directly. The step that uses it is given the approved markdown of one post and answers the HTML only.

**Configuration:** None. The skill carries no credentials, reads no settings and calls no tools.

**Development:** Clone the repository and run `node extension-kind-gate.mjs --package-root .` to validate the manifest. The bundle lives in `skills/blog-post-html/` and is one `SKILL.md` file.

**Troubleshooting:** If the published page shows raw markdown symbols, the HTML step did not receive this skill: check that the delivering extension declares its dependency on this package and that this package is installed. If the page's words differ from the approved text, the step did not follow the first rule; the publish card shows the HTML before anything is sent, so decline there.

## Works with

- Cinatra blog WordPress publish agent
- Any extension declaring a skill dependency on this package

## Capabilities

- Convert markdown headings, paragraphs, emphasis, links, lists, block quotes, code and images to plain HTML
- Keep every word of the post unchanged and in its order
- Keep heading levels and link addresses as written
- Turn a link or an image into HTML only when its address has one of the named safe forms (`http`, `https`, `mailto`, or an address inside the site that starts with `/`, `./`, `../` or `#`); any other address is written as plain text
- Escape the characters HTML needs escaped and add no script, style, wrapper, title or comment
- Answer with the HTML only
