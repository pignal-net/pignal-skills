---
name: knowledge-testing
description: |
  Knowledge testing craft for Pignal Flashcard sites. Q&A design, spaced
  repetition principles, and multi-level questioning. Use when creating
  flashcards, quiz questions, study materials, or knowledge assessments.
---

# Knowledge Testing Craft

A flashcard is not a note — it is a test. The act of retrieving an answer from memory is what creates durable learning. The craft is in designing questions that force productive retrieval, not questions that can be answered by pattern-matching or shallow recognition.

## Working with Your Pignal Site

1. Call `get_site_tools` to discover available capabilities
2. Call `get_metadata` to learn content types (card categories), workspaces (subjects/decks), and limits

---

## One Concept Per Card

The most important rule in flashcard design. A card that tests two concepts at once cannot diagnose which concept the learner knows and which they do not.

**Why this matters:** When a multi-concept card is answered wrong, the learner does not know what they got wrong — Concept A, Concept B, or both. They also cannot spaced-repeat the weak concept independently of the strong one. The card becomes an indivisible blob that is either "known" or "unknown," when the reality is more granular.

**The test:** Cover the answer. Can the question be answered with a single, specific piece of knowledge? If the answer requires combining two separate facts, split it into two cards.

**Example of violation:**
- Q: "What is the capital of France, and what is its population?"
- This tests geography and demographics — two different knowledge domains. Split it.

**Example of compliance:**
- Q: "What is the capital of France?" A: "Paris"
- Q: "What is the approximate population of Paris?" A: "~2.1 million (city proper)"

---

## Question Types

Different types of questions test different cognitive levels. A deck composed entirely of recall questions produces students who can recite facts but cannot use them. A good deck mixes question types deliberately.

### Recall Questions

**What they test:** Can the learner retrieve a specific fact from memory?

**When to use:** For foundational knowledge that must be automatic — definitions, names, dates, formulas, syntax.

**Design principles:**
- Be precise. "What does HTTP stand for?" has one right answer. "What is HTTP?" is too open for a flashcard.
- Avoid yes/no questions. "Is JavaScript dynamically typed?" can be guessed with 50% probability. "What type system does JavaScript use?" requires actual knowledge.

### Comprehension Questions

**What they test:** Can the learner explain a concept in their own words?

**When to use:** After the learner has memorized the fact and needs to deepen their understanding of it.

**Design principles:**
- Ask for explanation, not recitation. "Explain why database indexes improve read performance but slow writes" requires understanding. "What is a database index?" only requires recall.
- Accept paraphrasing. The answer to a comprehension question should capture the concept, not match a specific wording.

### Application Questions

**What they test:** Can the learner use knowledge to solve a new problem?

**When to use:** For skills that need to transfer to real-world situations.

**Design principles:**
- Present a scenario. "A user reports that the page loads slowly on mobile. Which performance metric should you check first and why?" requires applying knowledge of web performance to a specific situation.
- The scenario should be novel — not one the learner has seen before. If the scenario matches the learning material exactly, the learner is pattern-matching, not applying.

### Analysis Questions

**What they test:** Can the learner break down a system and identify relationships?

**When to use:** For developing diagnostic and reasoning skills.

**Design principles:**
- Present a complex situation. "Given this SQL query [query], explain why it performs a full table scan instead of using the index."
- The answer requires identifying the relevant components and their interactions, not just naming facts.

---

## Spaced Repetition

### Why Spacing Works

The human brain is not a hard drive — it is a prediction engine. It retains information that it predicts will be needed again and discards information it predicts will not be needed. The brain's primary evidence for "will be needed again" is "has been needed recently and repeatedly."

**The spacing effect (Ebbinghaus, 1885):** Information reviewed at increasing intervals is retained far longer than information crammed in a single session. Reviewing a card today, then in 3 days, then in 7 days, then in 21 days produces better retention than reviewing it four times today. This is one of the most replicated findings in cognitive science.

**Why cramming fails:** Massed practice (reviewing the same material many times in one session) produces short-term familiarity that feels like learning. The student feels confident. Then, days later, the information is gone. Spacing feels less effective in the moment — the student struggles more at each review — but the struggle is itself the mechanism that produces durable memory.

### Implications for Card Design

**Cards must be self-contained.** In a spaced repetition system, a card may appear days or weeks after the learner last studied related material. The question must make sense without context. "What about the second one?" assumes the learner just saw "the first one" — but they may not have.

**Cards must test, not remind.** A card that shows 90% of the answer and asks for the remaining 10% can be "answered" by recognition, not retrieval. Recognition decays faster than retrieval. The question must force the learner to produce the answer from memory.

**Difficulty should be calibrated.** A card that is always answered correctly is wasting review time — extend the interval. A card that is always answered incorrectly may be testing something the learner has not actually learned yet — teach it first, then test it.

---

## Distractor Quality

For multiple-choice flashcards and quiz questions, distractors (wrong answers) are as important as the correct answer. Bad distractors make the question trivially easy; good distractors force genuine knowledge discrimination.

### Why Distractors Matter

A multiple-choice question with one plausible answer and three absurd ones is not a test — it is a gift. The learner eliminates the obviously wrong options and selects the remaining one without actually knowing the answer. This produces the illusion of competence without the reality.

### Principles of Good Distractors

**Distractors should be plausible.** They should be wrong for a specific, identifiable reason — not randomly wrong. "What is the time complexity of binary search?" Distractors: O(n), O(n log n), O(1). Each is wrong but is the correct answer for a different algorithm, which means the learner must specifically know binary search's complexity to answer correctly.

**Distractors should target common misconceptions.** If students frequently confuse `==` and `===` in JavaScript, a question about `===` should include an option that describes `==`'s behavior. The distractor catches learners who hold the misconception.

**All options should be grammatically consistent.** If three options are noun phrases and one is a complete sentence, the outlier draws attention for the wrong reason.

**Avoid "all of the above" and "none of the above."** These options test test-taking strategy, not knowledge. A student who recognizes two correct options and sees "all of the above" can select it without evaluating the third.

---

## Reversible Cards

A reversible card can be tested in both directions: given the term, produce the definition, and given the definition, produce the term. These are two different cognitive tasks.

### When to Make Cards Reversible

**Vocabulary and terminology.** "Q: What is idempotent? A: An operation that produces the same result regardless of how many times it is executed." The reverse: "Q: An operation that produces the same result regardless of repetition is called ___. A: Idempotent."

**Symbol-meaning pairs.** "Q: What does the `&&` operator do in JavaScript? A: Logical AND." Reverse: "Q: Which JavaScript operator performs logical AND? A: `&&`."

### When Not to Reverse

**Explanatory answers.** If the answer is a paragraph-length explanation, the reverse card would require the learner to produce the exact question from a complex answer — which is not a useful cognitive task.

**One-to-many mappings.** If multiple terms map to the same definition, the reverse card is ambiguous. "Q: A data structure with FIFO ordering. A: ???" — it could be a queue, a pipe, a message buffer. Only reverse cards where the mapping is unambiguous.

---

## Cloze Deletion

Cloze deletion replaces a key word or phrase with a blank: "The CSS property `___` controls the space between an element's content and its border." Answer: `padding`.

### Why Cloze Cards Work

**Context aids retrieval.** The surrounding sentence provides semantic cues that help the learner access the right memory. This models real-world recall — you rarely need to produce a fact from zero context. You usually need to recall it within a surrounding framework.

**They test precision.** The learner must produce the exact missing piece, not a vague approximation. This is particularly effective for technical vocabulary, syntax, and formulas.

### Cloze Design Principles

**Delete the most meaningful word.** "The `padding` property controls the space between an element's ___ and its border" tests whether the learner knows "content." But "The `___` property controls the space between an element's content and its border" tests whether the learner knows the property name — which is more useful for a developer.

**One deletion per card.** Multiple deletions in one sentence test multiple concepts and create ambiguity about which blank is which. Split them.

**The surrounding context must be genuine.** Fabricating an artificial sentence just to create a cloze card produces a card that tests recall of the sentence, not recall of the concept.

---

## Bloom's-Aligned Card Design

A comprehensive flashcard deck should include cards at multiple Bloom's levels, progressing from recall to higher-order thinking.

| Level | Card Pattern | Example |
|-------|-------------|---------|
| Remember | "What is ___?" / "Define ___" | "What is the purpose of a foreign key?" |
| Understand | "Explain why ___" / "What is the difference between ___ and ___?" | "Explain why a hash table has O(1) average lookup time" |
| Apply | "Given [scenario], what would you ___?" | "Given an array of 1M records that needs frequent lookups by ID, which data structure should you use?" |
| Analyze | "Why does [system/code] behave this way?" | "This SQL query returns duplicates. Why?" |
| Evaluate | "Which approach is better for [scenario] and why?" | "For a write-heavy workload, is a B-tree or LSM-tree index more appropriate? Justify." |
| Create | "Design a ___" / "How would you build ___?" | "Design a caching strategy for an API with high read volume and infrequent writes" |

**Progression within a topic:** Start with Remember cards for foundational terms, add Understand cards for concepts, then Apply cards for practice. Analysis, Evaluation, and Creation cards come last and assume mastery of the lower levels.

---

## Common Flashcard Mistakes

**Cards that are too easy.** If the answer is obvious from the question, the card is not producing retrieval effort. No effort, no learning.

**Cards that are too vague.** "Tell me about JavaScript" — what aspect? What level of detail? Vague questions produce vague answers and test nothing specific.

**Cards that test recognition instead of recall.** Showing the answer and asking "is this correct?" tests recognition. Asking the learner to produce the answer tests recall. Recall is harder and more durable.

**Cards copied directly from source material.** If the question and answer are verbatim from a textbook, the learner may memorize the exact wording without understanding the concept. Paraphrase both question and answer.

**Too many cards on one topic.** Diminishing returns set in quickly. Five well-designed cards on a topic produce better retention than twenty mediocre ones, because the learner spends review time on quality retrievals rather than repetitive ones.

---

## The keySummary

The keySummary is the question or the card title. It must be self-contained and immediately comprehensible.

**Good patterns:**
- Direct question: "What is the difference between TCP and UDP?"
- Cloze-style: "The ___ protocol guarantees delivery order"
- Scenario: "Given a 503 error, what is the most likely server-side cause?"

**Anti-patterns:**
- Vague: "Networking concepts"
- Multi-part: "What is TCP, how does it differ from UDP, and when would you use each?"
- Leading: "Isn't it true that TCP guarantees delivery?"

---

## Workflow

1. **Discover** — `get_site_tools` then `get_metadata` for card types and workspaces (decks)
2. **Design** — One concept per card, choose question type by Bloom's level
3. **Draft** — `save_item` with question as keySummary, answer as content
4. **Review** — `validate_item` to mark as quality-checked
5. **Publish** — `vouch_item` with a descriptive slug
