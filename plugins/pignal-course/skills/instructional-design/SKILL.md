---
name: instructional-design
description: |
  Instructional design craft for Pignal Course sites. Bloom's taxonomy,
  learning objectives, lesson sequencing, and scaffolding. Use when
  creating tutorials, courses, lessons, or educational content.
---

# Instructional Design Craft

Teaching is not telling. Telling transmits information; teaching changes what a person can *do*. The craft of instructional design is in structuring content so that learning actually happens — not so that information is merely available.

## Working with Your Pignal Site

1. Call `get_site_tools` to discover available capabilities
2. Call `get_metadata` to learn content types (lesson categories), workspaces (courses/modules), and limits

---

## Bloom's Taxonomy

Benjamin Bloom's 1956 taxonomy (revised by Anderson and Krathwohl in 2001) describes six levels of cognitive complexity. It matters because most educational content operates at the lowest two levels — and most real-world performance requires the highest four.

### The Six Levels

**Remember** — Retrieve facts from memory. Recognize, recall, list, define.
This is the foundation. A student who cannot recall the parts of a cell cannot analyze how cells interact. But remembering is not understanding — a student can memorize the definition of "photosynthesis" without being able to explain what happens.

**Understand** — Construct meaning from information. Explain, summarize, paraphrase, classify.
This is where most educational content aims and stops. The student can explain a concept in their own words. But explanation is not application — understanding how a for-loop works is different from writing one.

**Apply** — Use knowledge in a new situation. Execute, implement, solve, demonstrate.
The first level that produces capability. The student can take what they learned in one context and use it in another. This requires practice, not just reading.

**Analyze** — Break material into parts and determine relationships. Compare, contrast, differentiate, organize.
The student can examine a system and identify its components, their relationships, and the principles governing them. Analysis produces insight.

**Evaluate** — Make judgments based on criteria. Critique, assess, justify, defend.
The student can look at a solution and determine whether it is good, and articulate why. This requires both domain knowledge and critical thinking.

**Create** — Produce something new. Design, construct, develop, formulate.
The highest level. The student can combine knowledge and skills to create original work. This is the ultimate goal of most education — not to reproduce what was taught but to generate what was not.

### Why Bloom's Levels Matter for Course Design

**Content that only operates at Remember/Understand produces students who can pass tests but cannot do anything.** If every lesson ends with "now list the five types of..." you are training recall, not capability. The lesson must progress through the levels.

**Assessment must match the level you are teaching.** If your lesson teaches analysis but your quiz tests recall, you are measuring the wrong thing. A student could pass the quiz without having learned the lesson's actual objective.

For the full taxonomy with verbs, examples, and strategies for each level, see [blooms-taxonomy.md](references/blooms-taxonomy.md).

---

## Learning Objectives

A learning objective states what the student will be able to do after completing the lesson. Not what they will "know" or "understand" — what they will be able to *do*. This distinction is the foundation of effective instructional design.

### Why Objectives Must Be Observable

"Understand recursion" is not a learning objective because you cannot observe understanding. "Write a recursive function that solves a base case and recursive case" is a learning objective because you can observe whether the student did it.

**The SMART Framework for Objectives:**

**Specific** — What exact skill or knowledge? "Implement pagination" not "Learn about data display."

**Measurable** — How will you verify it? The objective must describe an observable action. "Write," "build," "identify," "compare," "debug" are measurable. "Know," "appreciate," "understand" are not.

**Achievable** — Can the student reach this level in one lesson? If the objective requires knowledge from three prerequisite lessons, it belongs later in the sequence.

**Relevant** — Does this objective connect to the learner's real-world goals? A lesson on Git branching is relevant to a developer; a lesson on Git's internal object model may not be — unless the course is specifically about Git internals.

**Time-bound** — How long should the lesson take? This constrains scope and prevents lessons from trying to cover too much.

### Objective Examples by Bloom's Level

| Level | Weak Objective | Strong Objective |
|-------|---------------|-----------------|
| Remember | "Know the HTTP methods" | "List the four most common HTTP methods and their purposes" |
| Understand | "Understand CSS specificity" | "Explain why one CSS rule overrides another in a given example" |
| Apply | "Learn about error handling" | "Add try/catch blocks to an existing function that handles network failures" |
| Analyze | "Study database normalization" | "Identify normalization violations in a provided schema and explain the anomalies they cause" |
| Evaluate | "Know about testing approaches" | "Assess a test suite and identify which critical paths lack coverage" |
| Create | "Learn about API design" | "Design a REST API for a given domain that handles CRUD operations, pagination, and error responses" |

---

## Prerequisite Chains

Lessons do not exist in isolation. Each lesson depends on prior knowledge and enables future learning. The prerequisite chain is the sequence in which lessons must be taken to ensure each one has the foundation it needs.

### Why Sequencing Is Not Obvious

The expert's curse: when you know a subject deeply, the connections between concepts feel natural and bidirectional. You can approach recursion from multiple angles. But a novice needs one specific path — the path that builds each concept on the last without gaps.

**The dependency graph:** For each lesson, list what the student must already know to succeed. These are the hard prerequisites. Then list what would be helpful but not essential. These are soft prerequisites.

**Example dependency chain for web development:**
1. HTML structure (no prerequisites)
2. CSS selectors (requires: HTML structure)
3. CSS layout — Flexbox (requires: CSS selectors)
4. JavaScript basics (requires: HTML structure)
5. DOM manipulation (requires: JavaScript basics, CSS selectors)
6. Event handling (requires: DOM manipulation)
7. Fetch API (requires: JavaScript basics, event handling)

**Common sequencing mistakes:**
- **Circular dependencies.** Lesson A assumes knowledge from Lesson B, and Lesson B assumes knowledge from Lesson A. Resolve by extracting the shared foundation into a prior lesson.
- **Hidden prerequisites.** The lesson assumes the student knows something that was never explicitly taught. This is the most common cause of student frustration.
- **Over-sequencing.** Making every lesson depend on the previous one when some could be learned in parallel. Rigid sequences bore advanced students and create bottlenecks.

---

## Scaffolding

Scaffolding is the temporary support structure that allows a student to perform a task they cannot yet do independently. The scaffold is gradually removed as competence increases — like training wheels on a bicycle.

### Why Scaffolding Works

Vygotsky's "Zone of Proximal Development" describes the space between what a learner can do alone and what they can do with help. Learning happens in this zone — not in tasks that are too easy (no challenge) or too hard (no progress). Scaffolding keeps the task in the zone.

### Scaffolding Techniques

**Worked examples.** Show the complete solution to a problem, annotated with the reasoning at each step. The student sees not just what to do but *why* to do it. Then present a similar problem with less annotation. Then an unannotated problem. Then a novel problem.

**Why worked examples are powerful:** Cognitive Load Theory shows that novices learn more from studying worked examples than from solving problems, because problem-solving consumes working memory that could be used for learning. As expertise develops, the balance shifts — experts learn more from problem-solving.

**Partial completion.** Provide a partially completed exercise where the student fills in the critical parts. A function with the signature and comments written, where the student writes the body. A paragraph with the topic sentence provided, where the student writes the supporting details.

**Prompts and hints.** Rather than giving the answer, give a pointer toward it. "What data structure would let you look up values in O(1) time?" is a scaffold. "Use a hash map" is the answer — providing the answer skips the learning.

**Fading.** Systematically reduce support as the student demonstrates competence. Lesson 1: full worked example. Lesson 2: worked example with one step missing. Lesson 3: just the problem statement. This is not arbitrary — each lesson must verify that the student succeeded at the previous level before reducing support.

---

## Active Learning

Passive consumption (reading, watching, listening) produces recall at best. Active engagement (practicing, explaining, debating, building) produces capability.

### Why Passive Learning Feels Effective but Is Not

Reading a well-written explanation creates a feeling of understanding. The concepts make sense. The student nods along. Then they try to apply what they read and discover they cannot. This is the "illusion of competence" — a documented phenomenon in learning science. Passive consumption creates familiarity, not fluency.

**The fix:** Interrupt passive consumption with active tasks. After every major concept, the student must do something with it — answer a question, write code, solve a problem, explain it in their own words. These interruptions feel like friction, but they are where learning actually happens.

### Active Learning Techniques for Course Content

**Retrieval practice.** Ask the student to recall information from memory rather than re-reading it. A simple "What are the three types of CSS selectors?" after the section on selectors forces retrieval, which strengthens memory more than re-reading the section would.

**Elaboration.** Ask the student to connect new information to what they already know. "How is a CSS class selector similar to and different from an HTML attribute?" requires the student to build connections between concepts.

**Interleaving.** Mix different types of problems rather than grouping them by type. Instead of ten flexbox problems followed by ten grid problems, alternate between them. This is harder and feels less productive — but produces better long-term retention because the student must identify which technique to apply, not just execute the one they know is expected.

---

## Assessment Design

Assessment should measure whether the learning objective was achieved — nothing more and nothing less.

### Alignment Is Everything

If the objective says "Write a recursive function," the assessment must ask the student to write a recursive function. A multiple-choice question about recursion does not assess the objective — it assesses recall of facts about recursion, which is a different (lower) cognitive level.

### Assessment Types by Bloom's Level

| Level | Assessment Type |
|-------|----------------|
| Remember | Multiple choice, fill-in-the-blank, matching |
| Understand | Short answer explanation, concept mapping, paraphrasing |
| Apply | Exercises with novel problems, code challenges |
| Analyze | Case studies, comparison tasks, debugging exercises |
| Evaluate | Critique assignments, code review exercises, design reviews |
| Create | Projects, open-ended design tasks, portfolio pieces |

### Formative vs. Summative

**Formative assessment** happens during learning — quick checks, practice problems, self-tests. Its purpose is to identify gaps while there is still time to fill them. Formative assessment should be low-stakes (no grades, no judgment) because its function is diagnostic.

**Summative assessment** happens after learning — final projects, exams, certifications. Its purpose is to verify that the learning objective was met. Summative assessment is high-stakes and should be comprehensive.

---

## Chunking

Working memory holds approximately 7 plus or minus 2 items (Miller, 1956). This is a hard cognitive constraint that no amount of motivation overcomes. Course design must respect it.

### What Chunking Means for Lesson Design

**A lesson should teach one concept and its immediate applications.** If a lesson covers three unrelated concepts, working memory is overloaded and none of them will be retained well.

**Break complex topics into sequential chunks.** "How HTTP works" is too large for one lesson. "HTTP request structure," "HTTP response codes," "HTTP headers," and "HTTP methods" are four chunks that each fit in working memory and build on each other.

**Group related information.** Isolated facts are hard to remember. Facts organized into patterns or categories are easier because the pattern itself becomes a single "chunk" in working memory. Teaching HTTP status codes as a flat list of numbers is harder than teaching them as "2xx = success, 3xx = redirect, 4xx = client error, 5xx = server error" — four categories instead of dozens of codes.

---

## The keySummary

The keySummary is the lesson title. It must communicate what the student will learn and why it matters.

**Good patterns:**
- Outcome-focused: "Build a REST API with Node.js and Express"
- Question-framed: "How Does JavaScript Handle Asynchronous Code?"
- Skill-focused: "Debugging Memory Leaks in Production"

**Anti-patterns:**
- Vague: "Module 3: More About Databases"
- Internal numbering as titles: "Lesson 2.4.1"
- Too broad: "Everything About CSS"

---

## Workflow

1. **Discover** — `get_site_tools` then `get_metadata` for lesson types and workspaces
2. **Objective** — Write SMART learning objectives at the appropriate Bloom's level
3. **Sequence** — Map prerequisites; place the lesson in the chain
4. **Draft** — `save_item` with scaffolded content (concept, example, practice)
5. **Validate** — `validate_item` to mark as instructionally reviewed
6. **Publish** — `vouch_item` with a descriptive, learner-facing slug

For the complete Bloom's taxonomy reference with verbs and strategies, see [blooms-taxonomy.md](references/blooms-taxonomy.md).
