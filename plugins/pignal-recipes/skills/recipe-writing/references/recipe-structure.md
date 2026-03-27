# Recipe Structure Reference

Quick-reference for the standard recipe card format, ingredient conventions, and structured data principles.

---

## Standard Recipe Card Format

```
TITLE
  Concrete dish name. No "How to..." or "The Best..."

HEADNOTE
  1-3 sentences. Why this version, what to expect, any critical timing.

METADATA
  Prep: [active hands-on time]
  Cook: [heat/passive time]
  Total: [prep + cook + rest/chill]
  Yield: [concrete output with units]
  Difficulty: [beginner | intermediate | advanced]

DIETARY TAGS
  vegetarian, vegan, gluten-free, dairy-free, nut-free, etc.

EQUIPMENT (if non-obvious)
  - Items not found in every kitchen

INGREDIENTS
  Grouped by component (dough, filling, sauce, garnish).
  Listed in order of use within each group.

  Format: [quantity] [item], [preparation]
  Examples:
    200g all-purpose flour
    1 medium onion, finely diced
    2 cloves garlic, minced
    1 cup chicken stock, warm
    Salt and pepper to taste

INSTRUCTIONS
  Numbered steps.
  One action per step.
  Sensory cues + time estimates.
  Temperatures in both C and F.

NOTES
  Substitutions, storage, scaling, common mistakes.
```

---

## Ingredient Line Conventions

### The Rule: Quantity, Item, Preparation

The ingredient line tells the cook what to buy and how to prep it. The order reflects the workflow: get the thing, then do something to it.

| Write this | Not this | Why |
|---|---|---|
| 1 medium onion, diced | 1 cup diced onion | Onions are bought by count; dicing changes volume |
| 200g all-purpose flour | 1.5 cups flour | Weight is reproducible; cup measure varies 20% |
| 2 tablespoons butter, melted | 2 tablespoons melted butter | You measure butter solid, then melt it |
| 3 eggs, beaten | 3 beaten eggs | You crack first, then beat |
| 1 cup walnuts, toasted and chopped | 1 cup chopped toasted walnuts | Measure whole, then toast, then chop |

### When Volume is Acceptable

- Liquids: cups, tablespoons, teaspoons
- Small quantities of spices: 1/4 teaspoon, a pinch
- Items where precision does not matter: "a handful of herbs"

### When Weight is Required

- Flour, sugar, cocoa, powdered sugar (packing varies dramatically)
- Butter in baking (volume marks on wrappers are unreliable)
- Chocolate (chip volume vs bar weight differ)
- Cheese (shredded vs block volume differs)
- Meat and fish (always by weight)

### Sizing Conventions

| Term | Approximate weight |
|---|---|
| Small onion | 100g / 4oz |
| Medium onion | 150g / 5oz |
| Large onion | 200g / 7oz |
| Small clove garlic | 3g |
| Large clove garlic | 6g |
| Large egg (without shell) | 50g |
| Medium potato | 170g / 6oz |
| Large potato | 280g / 10oz |

---

## Time Estimation Guidelines

**Prep time includes:** washing, peeling, chopping, measuring, marinating (if under 30 min), assembling, bringing to room temperature.

**Cook time includes:** sauteing, roasting, baking, simmering, grilling, resting (if part of cooking process, e.g., meat resting).

**Excluded from both (note separately):** overnight soaking, multi-hour marinating, dough rising, freezing, chilling for set.

### Common Time Estimates

| Task | Time |
|---|---|
| Dice one onion | 3-5 min |
| Mince garlic (3 cloves) | 2 min |
| Preheat oven | 10-15 min |
| Saute onions until translucent | 4-5 min |
| Caramelize onions | 30-45 min |
| Bring large pot of water to boil | 8-12 min |
| Rest roasted meat | 10-20 min |
| Rice absorption method | 18-20 min |

---

## Nutrition Information

When including nutrition data:

- **Per serving**, using the yield stated in metadata
- Round to reasonable precision (not "243.7 calories" -- say "about 245 calories")
- At minimum: calories, protein, fat, carbohydrates
- Optionally: fiber, sodium, sugar, saturated fat
- Always note that values are estimates -- they vary with specific ingredients and portions

---

## Recipe Schema.org Principles

Pignal recipe sites generate structured data for search engines automatically. Understanding the schema helps you write content that maps cleanly to it.

**Key schema.org/Recipe properties and how recipe content maps to them:**

| Schema Property | Maps to |
|---|---|
| `name` | Recipe title |
| `description` | Headnote / keySummary |
| `prepTime` | Prep time (ISO 8601 duration: PT30M) |
| `cookTime` | Cook time |
| `totalTime` | Total time |
| `recipeYield` | Yield / servings |
| `recipeIngredient` | Each ingredient line (array of strings) |
| `recipeInstructions` | Each step (array of HowToStep) |
| `recipeCategory` | Item type (Main Course, Dessert, etc.) |
| `recipeCuisine` | Cuisine tag (Thai, Italian, etc.) |
| `keywords` | Tags |
| `nutrition` | NutritionInformation object |
| `suitableForDiet` | Dietary tags mapped to schema.org diet enums |
| `author` | Site owner / source attribution |

**What this means for writing:**

- The title should be a proper noun phrase, not a sentence
- The headnote's first sentence often becomes the meta description
- Ingredient lines should be self-contained strings (no "see above" references)
- Instructions should be discrete, numbered steps -- each becomes a HowToStep
- Dietary tags should use standard terms that map to `schema:RestrictedDiet` values

---

## Collection Organization

A well-organized recipe site uses Pignal's taxonomy features:

- **Item types** = course categories (Appetizer, Main Course, Side Dish, Dessert, Beverage, Snack, Condiment, Bread)
- **Workspaces** = curated collections (Weeknight Dinners, Meal Prep, Holiday, Summer Grilling)
- **Tags** = cross-cutting attributes (cuisine, method, diet, season, difficulty)

Types are structural. Workspaces are editorial. Tags are descriptive. A recipe is one type, may appear in multiple workspaces, and has several tags.
