# Mosaic Theory Weekly Briefing — Style & Methodology

## Writing Style

The Mosaic Theory weekly briefing discusses what we observed in HiddenState's signal data over the past week. It is written for VCs, hedge fund analysts, and policy professionals. The tone is that of a colleague walking you through what stood out in the data, what played out in the market, and where the interesting overlaps were.

### Tone & Voice

- **Conversational but sharp.** You are talking through the data with someone smart. Not pitching. Not advertising. Just pointing at interesting patterns and explaining why they matter.
- **Commit to the "so what."** Every signal discussed should end with a clear implication. Not "worth watching" or "worth monitoring." Say what it means. "If you are building document processing infrastructure on traditional OCR pipelines, this week's data suggests that bet is aging fast." Be direct. The reader should not have to do the translation from signal to implication themselves.
- **Modulate tone for serious signals.** Most patterns can be discussed with dry wit and detachment. But when the data shows something with genuine safety, security, or societal implications (e.g. systematic removal of safety refusals from frontier models, scientific peer review contamination), the tone should reflect the weight of the observation. Don't be preachy. But don't treat proliferation events with the same amused detachment as a product launch.
- **Short, declarative sentences.** Get to the point. Cut filler. Do not use em dashes. Split into two sentences instead.
- **Bold hooks.** Start paragraphs with a short bolded phrase that pulls the reader in. Vary the style throughout.

### Structure

The briefing has **three sections**:

1. **Cold Open** (2-3 sentences)
   - No greeting. Start with the most interesting thing from the week's data. What stood out? What pattern was unexpected? What's worth paying attention to?

2. **The Week in Data** (4-6 paragraphs)
   - Walk through what the data showed. Discuss the most interesting signals, mechanisms, and patterns that appeared in HiddenState's output over the past seven days. Not every signal. Just the juiciest ones.
   - For each signal worth discussing: what is the mechanism, why is it interesting, and what does it mean for the reader?
   - Where a signal was later confirmed by news (i.e. an announcement matched a mechanism we were already tracking, or a signal had `lead_days > 0`), mention it naturally. "We'd been tracking X for a few days before [outlet] reported Y." Don't force it. If the timing is interesting, say so. If it's not, just discuss the signal on its merits.
   - Where a strong signal has NO news confirmation yet, that's worth mentioning too. "This hasn't hit the press yet, but we're seeing convergence across multiple independent sources on [pattern]." These are often the most interesting items.
   - Mix it up. Some paragraphs about technical patterns, some about deals or funding, some about policy or regulation. Follow whatever the data actually surfaced that week.

3. **On Our Radar** (2-3 bullet points)
   - Short section at the end before the sign-off. List 2-3 signals that are building in the data but haven't confirmed yet. Things you're watching for next week.
   - Keep each to 1-2 sentences. No analysis, just the pattern and why it's interesting.
   - This gives readers a reason to come back next Monday. It also shows the data is alive and forward-looking, not just a retrospective.
   - Example: *"Seeing early convergence across multiple independent sources on [pattern]. Nothing public yet, but the trajectory is worth watching."*

4. **Sign-off**
   - HiddenState data attribution (italic): *"Signal data for this briefing is provided by [HiddenState](https://hiddenstate.io), Mosaic Theory's signal intelligence platform."*
   - Author sign-off: *"— Cosmo"* (or whichever author wrote the piece)

There is no rigid "Feature" section. If one signal dominates the week and deserves 3-4 paragraphs, give it a subheading and dig in. If the week was spread across many interesting signals, just discuss them in sequence. Let the data decide the shape of the post.

### What NOT to do

- Don't write a news digest. If a paragraph is just summarising a TechCrunch article, cut it.
- Don't advertise HiddenState. Don't write "HiddenState detected" or "our platform flagged" every other paragraph. Just discuss what the data showed. The attribution at the bottom does the job.
- Don't use newsletter formats ("Happy Monday", "Happening Today", "News In Brief").
- Don't use section-heading emojis.
- Don't write filler openers.
- Don't use em dashes.
- Don't cover every signal. Pick the 4-6 most interesting ones and do them justice.
- Don't end observations with "worth monitoring" or "worth watching" without saying what it means. That's a hedge, not an insight.
- Don't treat safety-critical developments (model safety removal, supply chain compromise, scientific integrity threats) with the same casual tone as commercial competition. Dry wit is fine. Trivialising is not.

***

## Weekly Blog Generation Methodology

This section describes how to produce a weekly Monday blog post from HiddenState archive data. An LLM should be able to follow these steps end-to-end.

### 1. Data Collection

- **Source**: `D:\hiddenstate\site\archive\` — JSON files named `YYYY-MM-DD.json`
- **Date range**: Read all JSON files from the **previous Monday through Sunday** (7 days). For a Monday March 23 post, read `2026-03-17.json` through `2026-03-23.json`.
- **Schema**: Each JSON file contains:
  - `top[]` — ranked signals with `mechanism`, `summary`, `w_index`, `w_breakdown`, `sources_detail[]`, `convergence_trail[]`
  - `announcements_feed[]` — curated news with `title`, `url`, `date`, `source`, `category`, `matched_mechanism`, `lead_days`
  - `stats` — metadata

### 2. Signal Selection

Read through all the `top[]` signals for the week. Pick the 4-6 most interesting based on:

- **Rich convergence trails**: Signals that appeared across multiple independent sources over several days are more interesting than one-day spikes.
- **Confirmed signals**: Any `announcements_feed` entry with `lead_days > 0` means the signal preceded a public announcement. These are worth discussing if the story is compelling.
- **Unconfirmed but strong**: A signal with high convergence but no matching news yet. These are often the most interesting to readers because nobody else is talking about them.
- **Commercial or policy relevance**: Signals that touch on funding rounds, M&A, regulatory moves, or competitive dynamics are more interesting to the audience than purely technical ones.

Don't force a "main feature." If one signal clearly dominates, give it more space. If not, discuss them at similar depth.

### 3. Source Linking Rules

**Do not reveal detection channels:**
- **DO link to**: arxiv papers, official company blog posts, news outlets (SiliconAngle, TheHackerNews, VentureBeat, TechCrunch), HuggingFace model pages
- **DO NOT link to**: Reddit threads, GitHub repositories, Hacker News posts, Papers with Code
- Filter `sources_detail` by type: accept `"rss"`, `"announce:*"`, `"huggingface"`; reject `"reddit"`, `"github"`, `"hn"`, `"papers_with_code"`
- If a claim only has Reddit/GitHub/HN sources, discuss the pattern without linking. The signal is the story.
- When describing source breadth, use: "independent sources", "research communities", "developer communities", "technical publications"

### 4. Content Generation

- **Frequency**: Weekly, covering the prior Mon-Sun
- **Voice**: First person plural ("we"). Conversational, not corporate. "We noticed", "the data showed", "what stood out this week", "we'd been tracking this for a few days when..."
- **Audience**: VCs, hedge fund analysts, policy professionals. But write for smart people, not for a specific job title.
- **Do NOT mention**: W-index scores, source counts by exact number, convergence trail mechanics by name, specific platform names as data sources
- **DO mention**: Rough timelines ("we'd been tracking this since Tuesday"), pattern descriptions ("multiple independent sources converged on the same mechanism"), confirmation when it happened naturally ("three days later, [outlet] reported the same thing")
- **Keep it natural**: The post should read like someone talking through their week's observations, not like a product demo or a press release.

### 5. Output

- Generate the blog post as HTML body content (everything inside `<article>`)
- Use `<h2>` for any subheading if one signal gets extended treatment (no emojis)
- Use `<strong>` for bold hooks at paragraph starts
- Use `<a href="...">` for source links (only approved sources per rule 3)
- Wrap in the blog post page template (nav, hero gradient, footer) from `d:\mosaictheory\blog\` existing posts

### 6. File Naming & Placement

- Save as `d:\mosaictheory\blog\YYYY-MM-DD.html` where the date is the Monday publication date
- Update `d:\mosaictheory\blog.html` listing page to include a card for the new post
