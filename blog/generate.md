# Weekly Blog Generation

Run this from Claude Code in the `d:\mosaictheory` directory. It will generate this week's blog post from HiddenState archive data.

## Instructions for Claude Code

When the user points you at this file, do the following:

### Step 1: Determine the date

Today is a Monday. The blog post covers the previous Monday through Sunday. Calculate those dates.

If today is not a Monday, use the most recent Monday as the publication date and cover the Mon-Sun before it.

### Step 2: Launch a subagent to generate the blog content

Launch one subagent with this prompt:

> Read the style guide at `d:\mosaictheory\format.md` (the full file, including the Weekly Blog Generation Methodology section). Then read the JSON archive files from `D:\hiddenstate\site\archive\` for dates [PREVIOUS MONDAY] through [PREVIOUS SUNDAY]. Follow the methodology exactly to produce the blog post content. Output the HTML `<article>` element, then on separate lines: `TITLE: [title]` and `SUMMARY: [one-line summary]`.

### Step 3: Red team the conclusions

Launch a second subagent to challenge the generated content. Give it the article text AND the same JSON archive files. Use this prompt:

> You are a red team reviewer. Your job is to check whether the conclusions in this blog post are actually supported by the underlying data.
>
> Read the JSON archive files from `D:\hiddenstate\site\archive\` for dates [PREVIOUS MONDAY] through [PREVIOUS SUNDAY]. Then read the blog post article below.
>
> For every claim that makes a jump from signal to conclusion, check:
>
> 1. **Is the pattern real?** Does the data actually show what the post says it shows? Check the mechanism names, source counts, convergence trails, and announcement feeds. If the post says "five independent sources converged," count them.
> 2. **Is the conclusion proportionate?** "Three labs shipped MoE models" is a fact. "Inference pricing collapses permanently" is a leap. Flag any conclusion that goes further than the data supports. Suggest a more defensible version.
> 3. **Is the lead time claim accurate?** If the post says "we tracked this N days before the announcement," check the convergence_trail dates against the announcements_feed dates. Is the number correct? Is it cherry-picked (e.g. the signal appeared once on day 1, disappeared, then reappeared on day 5)?
> 4. **Are there alternative explanations?** Could the "convergence" be coincidence, a shared upstream cause (e.g. a conference deadline), or an artifact of the data sources rather than a genuine independent pattern?
> 5. **Is anything stated as fact that is actually speculation?** Flag it.
> 6. **Does the "On Our Radar" section make defensible forward-looking claims?** Or is it just guessing?
>
> Output a numbered list of issues found. For each issue, quote the specific sentence, explain the problem, and suggest a fix. If a claim checks out, say so briefly. If the post is clean, say "No issues found."
>
> [PASTE THE GENERATED ARTICLE HTML HERE]

### Step 4: Fix issues

Review the red team output. For each flagged issue:
- If the claim is wrong, fix it.
- If the conclusion is disproportionate, tone it down to what the data supports.
- If a lead time claim is inaccurate, correct the number or remove the claim.
- If something is stated as fact but is speculation, reframe it as an observation or question.

Do not remove all edge from the writing. The point is not to make every sentence bulletproof. It is to make sure the sharp claims are actually supported and the speculative ones are framed as such. A post that hedges everything is useless. A post that makes unsupported leaps is worse.

### Step 5: Build the HTML page

Once the article is finalised:

1. Copy the template from the most recent post in `d:\mosaictheory\blog\` (the one with the latest date in its filename).
2. Replace:
   - The `<title>` tag with the new title
   - The `<meta name="description">` with the summary
   - The `<time>` element with the new date
   - The `<h1>` with the new title
   - The `<article>` block with the finalised content
3. Save as `d:\mosaictheory\blog\YYYY-MM-DD.html` using the Monday publication date.

### Step 6: Update the listing page

Add a new card at the top of the blog list in `d:\mosaictheory\blog.html`:

```html
<article class="blog-card">
    <time datetime="YYYY-MM-DD">Month DD, YYYY</time>
    <h2><a href="blog/YYYY-MM-DD.html">TITLE</a></h2>
    <p>SUMMARY</p>
    <a href="blog/YYYY-MM-DD.html" class="blog-read-more">Read &rarr;</a>
</article>
```

Insert it before the existing first `<article class="blog-card">`.

### Step 7: Final review

Read back the finalised post and check:
- No em dashes anywhere in the article body
- No links to Reddit, GitHub, or Hacker News
- No emojis in section headings
- No "Happy Monday" or newsletter-style greetings
- No "worth monitoring" or "worth watching" hedges without a clear implication
- HiddenState attribution line is present before the sign-off
- Author sign-off is present (default: "— Cosmo")
- "On Our Radar" section is present with 2-3 forward-looking signals

If any issues are found, fix them before finishing.
