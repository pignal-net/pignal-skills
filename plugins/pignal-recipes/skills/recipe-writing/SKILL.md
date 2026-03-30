---
name: recipe-writing
description: "Recipe writing craft for Pignal Recipe sites. Structured instructions, ingredient formatting, time calculations, and recipe structure. Use when writing recipes, formatting cooking instructions, managing a recipe collection, or operating a Pignal recipe site. Also use for food content and cooking guides."
allowed-tools: [mcp__*]
---

# Recipe Writing Craft

This skill covers the domain knowledge of writing good recipes -- the structure, conventions, and reasoning that make recipes reliable, reproducible, and useful. It applies when creating or editing recipe items on a Pignal recipe site via MCP tools.

---

## Recipe Structure

Every recipe follows a fixed order. The order exists because cooks read recipes in a specific sequence: they decide whether to make it, then shop, then prep, then cook.

### 1. Title

A recipe title names the dish, not the technique. "Caramelized Onion Tart" tells the cook what they will eat. "How to Caramelize Onions for a Tart" belongs in a blog post, not a recipe title.

Keep titles concrete. "Grandma's Special Soup" communicates nothing to someone who is not your grandma's grandchild. "Chicken Tortilla Soup with Charred Poblanos" tells the cook exactly what to expect.

### 2. Headnote

The headnote is the paragraph before the ingredient list. Its job is to answer: **why should I make this, and what should I expect?**

A good headnote covers:

- **What makes this version distinctive.** "This uses brown butter instead of melted butter, which adds a nutty depth." The cook needs to know why this recipe exists when thousands of versions are already published.
- **What the result tastes/looks/feels like.** "The crust shatters when you cut it, and the filling is barely set -- more custard than cake." This sets expectations so the cook does not pull it from the oven too early.
- **Any critical context.** "You need to start the dough the night before." If there is a time dependency that would ruin someone's dinner plans, say it here, not buried in step 7.

A headnote is not a personal essay. The cook is scanning to decide whether to commit. Respect their time.

### 3. Metadata Block

Before the ingredients, state:

- **Prep time** -- active hands-on work before cooking starts (chopping, measuring, marinating if short).
- **Cook time** -- time with heat applied or passive transformation (baking, simmering, resting).
- **Total time** -- prep + cook + any resting/chilling. If total differs from prep+cook (because of overnight steps), explain why.
- **Yield/servings** -- what this recipe produces. "Serves 4" is ambiguous. "Makes 4 bowls (about 1.5 cups each)" is useful. State yield in concrete units because "serving size" varies by person.

Why separate prep and cook time? Because a cook deciding at 5:30 PM what to make for 7 PM dinner needs to know that 45 minutes of cook time means hands-free oven time, not 45 minutes of active chopping.

### 4. Ingredient List

This is the most reference-heavy section. Cooks return to it repeatedly while working. Formatting matters enormously.

**Each ingredient line follows: quantity, preparation, item.**

- "2 cloves garlic, minced" -- not "2 minced garlic cloves"
- "1 cup walnuts, roughly chopped" -- not "1 cup of roughly chopped walnuts"

Why this order? Because the cook measures first, then preps. If you write "1 cup finely diced onion," they must dice before measuring, which changes the volume (diced onion packs tighter than whole). Write "1 medium onion, finely diced" instead -- they grab the onion, then dice it.

**Measurement principles:**

- **Weight over volume for dry ingredients.** "200g flour" is reproducible. "1.5 cups flour" varies by 20% depending on how the cook scoops. Use weight (grams preferred, as they are universal) for flour, sugar, butter, cheese, chocolate -- anything where packing varies.
- **Volume is fine for liquids and small quantities.** "1 cup milk," "2 tablespoons soy sauce," "1/4 teaspoon salt." Liquids measure consistently by volume. Small quantities are impractical to weigh.
- **Use common-sense units.** "1 medium onion" is better than "150g onion" because people buy onions by count, not weight. "3 eggs" not "150g eggs."
- **Specify size when it matters.** "1 large egg" matters in baking (large = ~50g without shell). "1 egg" is fine in a stir-fry.

**Group ingredients by use.** If a recipe has a dough and a filling, list them under subheadings. The cook should never have to scan the whole list to figure out which ingredients go where.

**List ingredients in order of use.** The cook reads top-to-bottom while working. If garlic goes in after onions, list onions first.

### 5. Equipment and Prerequisites

List anything non-obvious. Every kitchen has a knife and a pot. Not every kitchen has a 9-inch tart pan with a removable bottom, a candy thermometer, or parchment paper.

Why list equipment separately? Because the cook needs to know before starting whether they have what they need. Discovering mid-recipe that you need a food processor sends you scrambling.

Also list prerequisites: "Pie dough, chilled (from recipe above)" or "Beans, soaked overnight." These are not ingredients -- they are preconditions.

### 6. Instructions

**Number the steps.** Cooks lose their place. Numbered steps let them find where they left off. Bullet points do not.

**One major action per step.** "Dice the onion and saute until translucent, about 5 minutes" is fine -- those are sequential and fast. "Dice the onion, mince the garlic, julienne the carrots, make the sauce, and bring the pasta water to a boil" is five tasks crammed together. If you would do them at different times, they are different steps.

**Include sensory cues, not just times.** "Cook until the onions are soft and translucent, 4-5 minutes" is better than just "Cook 5 minutes." Times vary with stove power, pan size, and onion quantity. The sensory cue ("soft and translucent") is the real indicator; the time is an estimate to set expectations.

Why sensory cues matter: a cook at altitude, or with an underpowered burner, or using a different pan size will get different timing. "Until golden brown and fragrant" works everywhere. "6 minutes" only works under your specific conditions.

**State temperatures and positions.** "Preheat oven to 200C/400F, rack in the center." Many recipes forget the rack position, which matters for browning. Provide both Celsius and Fahrenheit because your audience is global.

**Front-load the verb.** "Whisk the eggs until foamy" not "Take the eggs and whisk them until they become foamy." Recipe writing is imperative and direct.

### 7. Notes Section

After the instructions, add notes for:

- **Substitutions.** "You can replace the heavy cream with full-fat coconut cream for a dairy-free version. The flavor will be slightly different but the texture holds." Explain what changes, not just what to swap.
- **Storage.** "Keeps in an airtight container in the fridge for 3 days. Reheat gently -- microwaving makes the crust soggy."
- **Scaling guidance.** If the recipe scales non-linearly (most baking does), say so. "This doubles well. Do not halve it -- the egg ratio will not work with less than one egg." Not all recipes scale linearly, and failing to mention this causes failures.
- **Common mistakes.** "If your caramel seizes, add a tablespoon of warm water and stir vigorously." Anticipating failure helps the cook recover.

---

## Dietary and Allergen Information

**Always tag dietary attributes when known:** vegetarian, vegan, gluten-free, dairy-free, nut-free.

Why? Because a cook with a dietary restriction should not have to read the entire ingredient list to determine if they can make this. Tags allow filtering at the collection level.

**Note allergens explicitly** when they are non-obvious. Soy sauce contains wheat (unless tamari). Worcestershire sauce contains anchovies. Thai curry paste often contains shrimp. The cook who is allergic to shellfish does not expect shrimp in their green curry paste.

---

## Recipe Scaling

When providing scaling guidance:

- **Spices and salt do not scale linearly.** Doubling a recipe does not mean doubling the salt. Increase by about 1.5x and adjust to taste.
- **Baking is chemistry.** Leavening (baking powder, yeast) and eggs scale unpredictably. A doubled cake recipe often needs less than double the baking powder. When in doubt, say "this recipe does not scale well -- make two batches instead."
- **Cooking times change with volume.** A doubled soup takes longer to come to a boil. A doubled batch of cookies takes the same time per sheet but more sheets.

---

## MCP Workflow for Pignal Recipe Sites

When operating a Pignal recipe site through MCP tools:

1. **Check site metadata first** (`get_metadata`). This tells you the configured item types, field limits, and vocabulary. Recipe sites typically have types like "Main Course," "Appetizer," "Dessert," "Side Dish."

2. **Use `list_items` to understand the existing collection** before adding new recipes. Check what cuisines, types, and difficulty levels are already covered. A good recipe collection has variety and coherence.

3. **Structure the recipe content in markdown.** Use the structure above: headnote paragraph, then `## Ingredients`, `## Instructions` (numbered list), `## Notes`. The keySummary field should be the headnote distilled to one sentence -- what makes this recipe worth making.

4. **Choose tags deliberately.** Tags on a recipe site serve as the cook's filtering system. Use consistent conventions: cuisine tags (thai, mexican, italian), method tags (grilled, braised, no-cook), diet tags (vegetarian, gluten-free), occasion tags (weeknight, meal-prep, holiday). Check existing tags with `list_items` to maintain consistency across the collection.

5. **Set appropriate visibility.** Draft recipes (testing phase) stay `private`. Proven recipes move to `vouched` for public listing. Untested but shareable recipes can be `unlisted` for limited sharing via link.

6. **Use workspaces for cookbook-style organization** when the collection is large. Workspaces like "Weeknight Dinners," "Baking," or "Holiday Favorites" give the site a curated, browsable structure beyond flat tag filtering.
