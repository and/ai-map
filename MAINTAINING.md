# Keeping AI Map up to date

AI Map goes stale in four ways, at different speeds. This guide says what to check, how often, and the rules for each kind of content, so every update keeps the page useful for someone completely new to AI.

| What goes stale | How it shows | How fast | When to check |
| --- | --- | --- | --- |
| Links | A resource moves, breaks or goes behind a login | Any time | Weekly, automatically |
| Resources | A link still works but is outdated, or a better beginner explainer exists | Months | Every quarter |
| Concepts | New ideas matter (agents, MCP), old ones fade or merge | 6–12 months | Twice a year |
| Perspectives | New roles appear (forward deployed engineer), others merge | About a year | Once a year |

All the content lives in one file, `index.html`, near the top of its `<script>`:

- `R` is the table of resources. Each entry is `[title, url, source, type]`.
- `REVIEWED` is the month shown in the footer as the last review.
- `BASICS` holds the six shared concepts.
- `FAMILIES` holds Build AI, Shape AI and Use AI, each with its perspectives (`personas`), concept groups and concepts.

A concept is `[id, name, one-line explanation, resource]`, where the resource is `R.someKey`, or `null` when there's no good beginner link yet. A `null` shows "No beginner link yet. Suggest one" on the page.

---

## 1. Weekly: links (automatic, about 5 minutes when something breaks)

The workflow `.github/workflows/check-links.yml` checks every link each Monday. It also checks whenever `index.html` changes. If anything is broken, the weekly run opens an issue titled **Broken links found**.

When that issue appears:

1. Open each broken link yourself. Some sites block automated checkers, so a link the checker flags (often with a 403 error) may work fine in a browser. If it does, close it out as a false alarm.
2. If the page moved, update the URL in `R`.
3. If it's gone, find a replacement that meets the resource rules in section 5. If you can't find one, set the concept's resource to `null`, which is better than a dead link.
4. Close the issue once it's fixed.

You can also run the check any time from the repo's **Actions** tab: choose **Check links**, then **Run workflow**.

## 2. Quarterly: resources (about 1 hour)

Work through this list:

- [ ] **Triage issues and suggestions.** Use the resource rules in section 5 to decide on each suggested link.
- [ ] **Fill gaps.** Search `index.html` for `null` and try to find a beginner link for each one. Known gaps are listed in section 8.
- [ ] **Upgrade weak links.** Replace links that point to a whole YouTube channel or a docs home page with the specific video or page that explains the concept. Replace reused links (one resource serving several concepts) with dedicated ones where you can.
- [ ] **Check age.** In fast-moving areas (models, tools, agents, regulation, prices), replace any resource older than about two years if something newer and just as clear exists.
- [ ] **Spot-check five links in a private browser window.** Confirm there's no login wall, no paywall and no new cookie wall that blocks reading.
- [ ] **Update `REVIEWED`** to the current month and year.

## 3. Twice a year: concepts (about 2–3 hours)

For each perspective, ask:

1. **Would someone in this role need this now?** Look at five or so current job descriptions, course syllabi or conference talk lists for the role. Concepts that keep appearing belong. Concepts nobody mentions any more are candidates to merge or remove.
2. **Is anything big missing?** Skim the last six months of a few reliable newsletters and blogs, such as the Towards AI newsletter, The Batch from DeepLearning.AI, Simon Willison's blog, and the Hugging Face blog. A term that keeps coming up for this audience is a candidate.
3. **Are the explanations still true?** The one-liners should match how things work today. For example, "what happens to what you share" changes as companies change their data policies.
4. **Is it still 3 groups of about 3?** Keep perspectives easy to scan. If one grows past 12 concepts, merge or split it.

Then check the six basics. They should still be the ideas every perspective builds on, written for someone who has never used AI.

## 4. Yearly: perspectives (about half a day)

- **New roles.** Add a perspective when a role has its own question to ask of AI and at least six concepts not already covered elsewhere. Forward deployed engineer is a likely next one, sitting in Build AI after AI engineer.
- **Merging.** If two perspectives now share most of their concepts, merge them.
- **Families.** Keep the three families unless a role genuinely fits none of them:
  - **Build AI** is for people who make models, or software on top of models. Order the perspectives from the models themselves to software built on them.
  - **Shape AI** is for people who decide what gets built, how it behaves and what rules it follows.
  - **Use AI** is for people who get things done with AI tools without writing code.
- **Related links.** Check that each perspective's `related` list still points to the most useful neighbours.

---

## 5. Rules for resources

Every concept gets **one** link, chosen for someone who has never heard the term. A good resource is:

- **Free and open:** no login, no paywall, no trial sign-up.
- **Beginner level:** it explains the idea in plain language before any jargon. If you need to already know the concept to follow it, it's the wrong link.
- **Specific:** a single article, video or chapter about this concept, not a channel, home page or whole course, unless nothing more specific exists.
- **Short:** readable or watchable in about 20 minutes or less.
- **Reputable and stable:** a well-known publisher, university, standards body, government site or respected practitioner. Prefer evergreen explainers over news stories.
- **Current:** updated within about two years for fast-moving topics.
- **Not a sales page:** vendor pages are fine when they're documentation or a neutral explainer, not marketing.
- **Varied:** avoid one publisher dominating a perspective.

Set the `type` honestly, because the page uses it to tell the reader what they're opening. "Video" shows as "Watch", "Course" as "Take the course", "Tool" as "Try", and anything else as "Read".

To add a resource, add an entry to `R` with a short camelCase key, then point the concept at it:

```js
ibmTokens: ["What are tokens?", "https://example.com/tokens", "IBM", "Article"],
...
["tokens-context", "Tokens and the context window", "Models read text in small chunks...", R.ibmTokens],
```

## 6. Rules for concepts

- **Name:** five words or fewer, in plain words.
- **Explanation:** one sentence of about 15 words or fewer. Don't use jargon the basics haven't explained.
- **Id:** short, lowercase, words joined with hyphens, and unique across the whole page. **Never change an existing id.** Visitors' ticks are saved against ids, so a changed id silently clears their progress. To reword a concept, change its name and explanation and keep the id.
- **Removing a concept:** delete it. Its old ticks are simply ignored. Don't reuse its id for something else.

## 7. Rules for perspectives

Add an object to the right family's `personas` array, in the position that fits the family's order:

```js
{ id: "forward-deployed", name: "Forward deployed engineer",
  question: "How do I make AI work inside a customer's real systems?",
  who: "Engineers who embed with customers to build and adapt AI solutions on site.",
  groups: [
    { name: "...", items: [ ["fde-concept-id", "Name", "One-line explanation.", R.someResource], ... ] },
    ...
  ],
  links: [R.someResource],            // "Go further" links
  related: ["ai-engineer", "product"] // ids of neighbouring perspectives
}
```

- `id` is used in links (`#forward-deployed`). Keep it short and permanent.
- Add the new id to the `related` lists of its neighbours too, so the link works both ways.
- The tabs, cards and progress counts update by themselves. The sentence "This map shows nine perspectives" in the introduction and the README table do not, so update those.

## 8. Known gaps (as of November 2026)

**No beginner link yet (`null`):** benchmarks and their limits; MLOps; cost and speed per use; build, buy or partner; total cost of ownership; misuse and dual use; meeting notes and follow-ups; your company's AI policy; being open about AI use; mixing AI with your own work; licensing and commercial use.

**Points to a whole channel or docs home, not a specific page:** 3Blue1Brown, Andrej Karpathy and StatQuest (YouTube channels), Two Minute Papers, ElevenLabs docs, Ollama docs, Arize Phoenix docs, Anthropic Academy, Anthropic prompt library.

**One resource reused for several concepts:**
- Ethan Mollick's "15 times to use AI" (3 concepts)
- Ethan Mollick's "good enough prompting" (3)
- StatQuest (3)
- Ethan Mollick's "Using AI right now" (2)
- PAIR "User needs" (2)
- AI For Everyone (2)
- NIST AI RMF (2)
- Content Credentials (2)
- Learn Prompting (2)
- IBM's hallucinations explainer (2)

Update this list as you fix things.

## 9. Publishing a change

1. Edit `index.html`, and `README.md` if the perspectives changed.
2. Open `index.html` in a browser. Check that the page loads, that the browser console shows no errors, that the tab or perspective you changed looks right, and that it works at phone width (use the browser's device toolbar).
3. Commit and push to `main`. GitHub Pages redeploys within a minute or two, and the link check runs on the push.

## 10. Getting an AI assistant to help

These prompts work with Claude or another assistant that can browse. Paste the prompt together with the relevant part of `index.html`. Always open the links it suggests yourself before adding them.

**Quarterly resource review**

> Here is the resource table (`R`) and concept list from my AI Map page, which teaches AI concepts to complete beginners. For each concept whose resource is `null`, a whole channel, a docs home page or a reused link, suggest one replacement that meets these rules: free, no login, plain-language for someone who has never heard the term, specific to the concept, under 20 minutes, from a reputable source, and updated within two years for fast-moving topics. Open each page to confirm it loads and fits. Give me the result as `R` entries I can paste in, and say which concept each one is for.

**Twice-yearly concept review**

> Here is the [perspective name] perspective from my AI Map page: its question, who it's for, and its concepts with one-line explanations. Using current job descriptions, course syllabi and the last six months of AI news, tell me which concepts are now missing, which have faded or merged, and which explanations are no longer accurate. Keep each perspective to about 3 groups of 3 concepts. Don't change concept ids. Suggest new ones in the same format.

**Yearly perspective review**

> Here are the three families and nine perspectives on my AI Map page. Which real roles have emerged or grown in the past year that have their own question to ask of AI and at least six concepts not covered here? For each, say which family it belongs in and draft it in the same format, including one beginner resource per concept that you've opened and checked.
