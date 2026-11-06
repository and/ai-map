# AI Map

A map of AI for people new to it. AI looks different depending on where you stand, so the map shows it from nine perspectives. Each one lists the concepts that matter from that view, with a checklist to track what you know.

**Live page:** https://and.github.io/ai-map/

## The perspectives

Everyone starts with six shared basics: what AI, machine learning and LLMs are, training vs using a model, tokens and context windows, why models make things up, prompting, and what happens to the data you share.

| Family | Perspective | The question it asks |
| --- | --- | --- |
| Build AI | Researcher | How do models work inside, and how do we make them better? |
| Build AI | ML engineer and data scientist | How do I train, adapt and run models myself? |
| Build AI | AI engineer | How do I build reliable products on top of existing models? |
| Shape AI | Product and design | What should we build with AI, and how will people experience it? |
| Shape AI | Business leader | Where does AI create value for us, and what are the risks? |
| Shape AI | Safety, ethics and policy | How do we make AI safe, fair and accountable? |
| Use AI | Everyday user | How can AI help me day to day, and when can I trust it? |
| Use AI | Professional at work | How do I use AI to do my job better? |
| Use AI | Creator | How do I make images, video, music and writing with AI? |

The page opens on three tabs: Build AI, Shape AI and Use AI. Choosing one shows the perspectives inside it, and choosing a perspective opens its concepts. Each perspective has nine concepts in three groups, and links to related perspectives. That's 87 concepts in all. Every concept has a one-line explanation and one beginner-friendly link to learn it from.

## How it works

It's one `index.html` file, with no build step and no dependencies apart from Google Fonts. Ticks are saved in your own browser's local storage and never leave it. To link to a tab or a perspective, add its id to the URL, for example `#build` or `#researcher`.

## Adding a perspective

Perspectives live in the `FAMILIES` list near the top of the script in `index.html`. To add one, such as a forward deployed engineer, add an object to a family's `personas` array with:

- `id`: a short URL-safe name, used in links (`#forward-deployed`)
- `name`, `question` and `who`
- `groups`: an array of `{ name, items }`, where each item is `[id, name, one-line explanation]`. Concept ids must be unique across the page.
- `links`: starter resources as `[title, url]`
- `related`: ids of other perspectives to suggest

The tabs, cards and progress counts update by themselves.

## Contributing

Suggestions for perspectives, concepts or better beginner resources are welcome as issues or pull requests. [MAINTAINING.md](MAINTAINING.md) explains how the content is organised, the rules for resources, concepts and perspectives, and the review routine that keeps the page current. A weekly GitHub Action checks every link and opens an issue if any are broken.

## Credits

Some resources were picked from Louis-François Bouchard's [start-ai-engineering](https://github.com/louisfb01/start-ai-engineering) roadmap.
