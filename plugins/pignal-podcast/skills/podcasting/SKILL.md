---
name: podcasting
description: |
  Podcasting craft for Pignal Podcast sites. Show notes structure, episode management, guest attribution, timestamps, and series planning. Use when managing podcast episodes, creating show notes, or planning series.
allowed-tools: [mcp__*]
---

# Podcasting Craft

Podcasting craft for Pignal Podcast sites. Show notes structure, episode management, guest attribution, and series planning. Use when managing podcast episodes, writing show notes, planning episode series, or operating a Pignal podcast site.

---

## The MCP Workflow

Every podcast episode operation follows this sequence. Do not skip steps — each one prevents a different class of mistake.

1. **Discover** — `get_site_tools` to learn what the site can do.
2. **Learn the site** — `call_site_tool` with `get_metadata` to get the site's types, workspaces, limits, and quality guidelines. This is not optional. A podcast site may have types for full episodes, bonus content, trailers, and announcements — each with different structures and expectations.
3. **Plan** — Decide content type, workspace (often maps to a show or series), tags, and structure based on what `get_metadata` returned.
4. **Draft** — Write the keySummary (episode title) and markdown content (show notes) following the craft principles in this skill.
5. **Create** — `call_site_tool` with `save_item` to save the episode entry.
6. **Validate** — `call_site_tool` with `validate_item` to apply a quality action. For podcasts, validation confirms that timestamps are accurate, guest attribution is correct, and resource links are live.
7. **Publish** — `call_site_tool` with `vouch_item` to set visibility to `vouched` with a clean URL slug.

---

## Show Notes Structure

Show notes are the written companion to an audio episode. They serve three audiences simultaneously: listeners who want to reference what they heard, potential listeners deciding whether to press play, and search engines that cannot index audio.

### Why Show Notes Matter

**Audio is unsearchable.** A brilliant 45-minute conversation about distributed systems is invisible to Google, to internal site search, and to AI assistants — unless the show notes capture its substance in text. Show notes are the bridge between audio content and discoverability.

**Listeners need reference material.** During an episode, a guest mentions a tool, a book, a technique. The listener is driving, walking, or working out. They cannot pause to look it up. Show notes are where they return to find those references — and if the references are not there, the value of mentioning them in the episode is lost.

**Potential listeners scan before committing.** Podcast episodes are a significant time investment — 30-60 minutes is common. A reader scanning show notes is asking: "Is this episode worth my time?" Good show notes answer that question in 30 seconds.

### The Show Notes Arc

Every episode's show notes should follow this structure:

**1. Episode Summary (2-4 sentences)**

A concise description of what the episode covers and why it matters. This is the most important part of the show notes — it appears in podcast apps, RSS readers, and search results.

Why brevity matters here: Podcast apps display 2-3 lines of description before truncating. If the summary doesn't hook the potential listener in those lines, the episode gets scrolled past. Write the summary as if the reader will see only the first sentence.

**Principles:**
- Lead with the topic, not the guest. "How to build resilient distributed systems — with Jane Chen, Staff Engineer at Stripe" is better than "In this episode we talk to Jane Chen about various topics."
- Name the specific value. "Three strategies for handling partial failures in microservices" beats "We discuss distributed systems."
- Match tone to the episode. If the conversation was casual and exploratory, don't write show notes that sound like a white paper.

**2. Timestamps**

A list of key moments in the episode with their timecodes. Timestamps transform a linear audio experience into a navigable document.

Why timestamps matter: They serve double duty. For returning listeners, timestamps are a table of contents — "I remember they talked about caching, let me jump to that part." For new listeners, timestamps are a preview — the topics listed signal whether the episode is relevant.

**Timestamp principles:**
- Use `HH:MM:SS` or `MM:SS` format consistently. Pick one and never mix them.
- One timestamp per distinct topic or significant shift. Over-timestamping (every 30 seconds) is noise. Under-timestamping (3 timestamps for a 60-minute episode) is unhelpful.
- Write timestamps as topic labels, not sentence fragments. "14:32 — Why connection pooling breaks in serverless" is scannable. "14:32 — Talking about connections" is vague.
- Aim for one timestamp every 3-7 minutes of audio. A 45-minute episode typically warrants 8-15 timestamps.

**3. Key Takeaways (3-5 bullet points)**

The most important ideas or actionable insights from the episode, extracted and condensed.

Why takeaways matter: They serve listeners who won't relisten and readers who won't listen at all. A well-written takeaway captures the value of a 5-minute discussion in a single sentence. This is not a transcript — it is a distillation.

**Principles:**
- Each takeaway should be self-contained and actionable.
- Attribute ideas to the speaker: "Jane argues that circuit breakers should be the first resilience pattern you implement, not the last."
- Prioritize surprising or counterintuitive insights over expected ones.

**4. Resources and Links**

Everything mentioned in the episode — tools, articles, books, people, companies — collected in one place with links.

Why a resources section matters: This is the most practically useful part of show notes. A listener who enjoyed the episode will scan this section looking for things to explore further. If the mentioned resources aren't here, the listener has to scrub through the audio to find the moment where the name was said, which most won't do.

**Principles:**
- Link to the most useful destination, not just the homepage. If the guest mentioned a specific blog post, link to that post — not the blog's main page.
- Group resources by type if the list is long: Tools, Articles, Books, People.
- Include brief context: "Jepsen (distributed systems testing tool) — mentioned at 23:15" helps the listener remember why it was referenced.

**5. Guest Bio**

A brief, respectful biography of the guest that establishes their credibility on the episode's topic.

Why guest bios matter: The guest's background is the reason the audience should trust their perspective. A bio that says "Jane Chen is a software engineer" is generic. A bio that says "Jane Chen is a Staff Engineer at Stripe, where she leads the distributed payments team. She previously built fault-tolerant systems at Netflix and has spoken at QCon and Strange Loop about resilience patterns" establishes exactly why her opinions on distributed systems carry weight.

**Principles:**
- Focus on relevance to the episode topic, not a complete career history.
- Include current role, relevant past experience, and any public work (talks, writing, open source).
- Link to the guest's website, social profile, or relevant work.
- 2-4 sentences maximum. A guest bio is context, not a biography.

---

## Episode Titles (The keySummary)

The episode title appears in podcast apps, the site feed, search results, and RSS. It is the primary mechanism for attracting listeners.

### Titling Principles

**Describe the value, not the format.** "How to Debug Distributed Systems Under Pressure" tells the listener what they'll learn. "Episode 47: Chat with Jane" tells them nothing.

Why this matters: In a podcast app, your episode competes with hundreds of others in the listener's queue. The title is the only information they see before deciding to press play or skip. A title that communicates specific value wins attention.

**Include the guest name when the guest is the draw.** "Designing for Failure — with Jane Chen (Stripe)" works because the guest's name and company add credibility. For solo episodes or lesser-known guests, the topic should carry the title alone.

**Episode numbers: useful but not leading.** "Ep. 47: How to Debug Distributed Systems Under Pressure" is fine. "#47 — How to Debug Distributed Systems Under Pressure" is also fine. "Episode 47" alone as a title is never fine — it tells the listener nothing about value.

**Avoid one-word or vague titles.** "Resilience" could be about anything. "Building Resilient Systems That Survive Black Friday Traffic" is specific enough to attract the right listener and repel the wrong one — both desirable outcomes.

### Series Titling

For episodes that belong to a series, use consistent prefixes that signal the series while still describing the individual episode:

- "Edge Computing Deep Dive, Part 1: Why Latency Matters More Than Throughput"
- "Edge Computing Deep Dive, Part 2: Deploying Workers to 300 Cities"

This pattern lets the listener understand both the series context and the specific episode's content from the title alone.

---

## Series Arc Planning

A podcast series (a sequence of related episodes) benefits from planning the arc before recording episode one.

### Why Plan the Arc

**Prevents repetition.** Without a plan, episode 3 often covers ground from episode 1 because the host forgets what was already discussed. An arc plan assigns specific territory to each episode.

**Creates anticipation.** When listeners know that a series has a defined arc — "this is Part 2 of 5, and Part 3 covers deployment" — they are more likely to subscribe and return. Open-ended "we'll see how many episodes this takes" signals lack of preparation.

**Enables callbacks.** A planned arc can reference future episodes ("we'll go deeper on this in Part 3") and past ones ("as we covered in Part 1"). These callbacks create a sense of coherent knowledge-building that keeps listeners engaged across episodes.

### Planning the Arc

1. **Define the series thesis.** What should a listener understand after completing the entire series? This is the macro-level takeaway.
2. **Break the thesis into 3-7 episodes.** Each episode should cover one distinct subtopic that contributes to the thesis. Fewer than 3 doesn't justify a series; more than 7 tests listener commitment.
3. **Sequence logically.** Start with foundations, build to complexity, end with synthesis. A listener should be able to follow the series in order without confusion.
4. **Assign each episode a clear scope.** What is covered AND what is explicitly deferred to a later episode. This prevents scope creep within individual episodes.
5. **Plan the finale.** The last episode should synthesize the series — not just cover "the remaining topics." It should answer the thesis question from step 1 and reference insights from earlier episodes.

---

## Guest Attribution

Guests contribute their expertise, time, and reputation to your show. Attribution should reflect that contribution.

### Attribution Principles

**Always credit the guest in the episode title or subtitle.** The guest's name appearing in the title signals their importance and helps with discoverability — their audience may search for them by name.

**Use the guest's preferred name and title.** Ask before publishing. "Dr. Jane Chen" vs. "Jane Chen" vs. "Jane" — the guest's preference matters. Getting it wrong signals carelessness.

**Link to the guest's chosen platform.** Some guests prefer Twitter, some prefer LinkedIn, some prefer their personal site. Link to what they actively maintain, not what you found first.

**Credit ideas to their source.** In show notes, when summarizing a point the guest made, attribute it: "Jane argues that..." or "According to Jane's experience at Stripe..." This is both respectful and useful — it helps the reader weigh the claim based on the source's authority.

---

## Listener Engagement

### Calls to Action

Every episode should end with one clear call to action — not three. Multiple CTAs dilute effectiveness because the listener is choosing between actions instead of simply acting.

**Effective CTAs:** Subscribe, leave a review, visit the show notes for links, join the community, share with a colleague who would benefit.

**Rotate CTAs across episodes.** "Subscribe" in episode 1, "leave a review" in episode 5, "share this episode" in episode 10. This prevents the listener from tuning out a repeated request.

### Building Community

**Reference listener contributions.** If listeners send questions, suggest topics, or share feedback, acknowledge them by name (with permission). This creates a feedback loop — listeners contribute more when they see contributions recognized.

**Create referenceable show notes.** When listeners share your podcast, they often share the show notes page, not the audio file. Show notes that stand alone as useful content — with takeaways, timestamps, and links — make sharing feel valuable.

---

## Episode Metadata

### Tags

Tag episodes by topic, guest, and series. This creates three navigational paths through the archive:

- **Topic tags** ("distributed-systems," "devops," "leadership") let listeners find episodes about subjects they care about.
- **Guest tags** (the guest's name, lowercased and hyphenated) let listeners find all appearances by a specific guest.
- **Series tags** ("edge-computing-deep-dive") let listeners find all episodes in a series.

Aim for 3-5 tags per episode. Check existing tags via `list_items` or `search_items` before creating new ones — consistency is more important than precision.

### Workspaces for Shows

If the site hosts multiple shows or distinct series, use workspaces to organize them. Each workspace appears as a separate section on the site, making it easy for listeners to browse a specific show's episodes.

### Slugs

Episode slugs should describe the content, not the number. `debugging-distributed-systems-jane-chen` is a better slug than `ep-47` because:
- It contains searchable keywords.
- It communicates value in the URL itself (useful when shared on social media where only the URL may be visible).
- It remains meaningful if the episode numbering ever changes.

---

## Publishing Workflow for Episodes

1. **Create the episode entry** — `save_item` with the show notes as content and the episode title as keySummary.
2. **Validate** — `validate_item` to confirm timestamps, links, and guest attribution are correct. For podcasts, this is the "producer review" step — ensuring the written companion matches the audio.
3. **Set the slug** — When publishing with `vouch_item`, provide a descriptive slug. If part of a series, include the series identifier: `edge-computing-part-2-deploying-workers`.
4. **Tag appropriately** — Apply topic, guest, and series tags.

For batch publishing (e.g., launching a backlog of episodes), use `batch_vouch_items` to publish multiple episodes at once. Consider staggering publication dates if the site supports scheduling — dropping 10 episodes at once gives listeners no reason to return regularly.
