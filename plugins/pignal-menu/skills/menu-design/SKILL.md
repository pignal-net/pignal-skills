---
name: menu-design
description: |
  Menu engineering for Pignal Menu sites. Menu psychology, pricing display,
  food description writing, section organization, and dietary information.
  Use when creating menus, price lists, food catalogs, managing restaurant
  or cafe content, or operating a Pignal menu site. Also triggers on
  pricing lists, food menus, beverage lists, or any itemized service pricing.
allowed-tools: [mcp__*]
---

# Menu Engineering

A well-designed menu isn't just a list of items and prices — it's a sales tool. Menu engineering combines psychology, design, and clear communication to help customers make satisfying choices while supporting business goals. The craft is in making the menu easy to navigate, the descriptions appetizing, and the pricing feel fair.

## Working with Your Pignal Site

1. Call `get_site_tools` to discover available capabilities
2. Call `get_metadata` to learn content types (item categories), workspaces (menu sections), and limits

## Menu Psychology

### The Golden Triangle

Eye-tracking studies consistently show that readers scan menus in a predictable pattern: center first, then upper right, then upper left. This is why restaurants place high-margin items in the center of each section — it's where eyes land first.

**Application in Pignal:** The items that appear first in each workspace/section get the most attention. Place your best items (highest quality, best margin, most representative) at the top.

### The Paradox of Choice

More options means harder decisions. Research shows that 7-10 items per section is the sweet spot — enough variety to satisfy different preferences, few enough to avoid decision paralysis.

**Why this matters:** Customers overwhelmed by choice often default to the cheapest option or skip the section entirely. Curating your menu drives better experiences.

## Pricing Display

### Drop the Dollar Sign

Research from the Cornell School of Hotel Administration found that removing currency symbols increases spending. "$12.00" feels more expensive than "12" — the dollar sign triggers pain-of-paying associations.

**In Pignal:** When writing prices in content, use plain numbers: "12" or "12.00" rather than "$12.00."

### Charm Pricing

Prices ending in .95 or .99 signal value. Prices ending in .00 signal quality. Choose based on your positioning:
- Casual/value dining: 9.95, 12.99
- Fine dining/premium: 15, 24, 38

### The Decoy Effect

Placing a high-priced item near your target item makes the target feel more reasonable. A $45 steak next to a $28 steak makes the $28 feel like a deal — even if it's your most expensive item otherwise.

### Price Placement

Don't align prices in a column down the right side — it creates a "price runway" where customers scan prices instead of descriptions. Embed the price at the end of the description, flowing naturally.

## Food Description Writing

### Sensory Language

Descriptions that engage the senses sell better than factual lists:

- **Flat:** "Grilled chicken breast with vegetables"
- **Sensory:** "Herb-crusted chicken breast, chargrilled over oak, served with roasted seasonal vegetables"

**The key sensory triggers:**
- **Taste:** tangy, smoky, sweet, umami, bright
- **Texture:** crispy, velvety, tender, crunchy, silky
- **Temperature:** chilled, warm, sizzling
- **Preparation:** slow-roasted, hand-rolled, stone-fired, cold-pressed

### Conciseness

2-3 lines maximum per item. The description should create appetite, not read like a recipe. Lead with the most appealing element.

### Naming Matters

Descriptive names outsell generic names:
- "Chicken Sandwich" → "Tuscan Herb Chicken on Ciabatta"
- "House Salad" → "Market Garden Salad with Citrus Vinaigrette"

But don't oversell — the name must match the plate. Creative names that mislead destroy trust.

## Section Organization

### Logical Flow

Traditional meal flow: Starters → Mains → Sides → Desserts → Beverages

But consider your context:
- Cafes: Beverages first (that's why people come)
- Bars: Drinks by category (cocktails, wine, beer, non-alcoholic)
- Bakeries: By occasion (breakfast, lunch, celebration)

### Section Naming

Clear, descriptive section names help navigation:
- "Small Plates" is clearer than "Antipasti" (unless your audience expects Italian)
- "For the Table" communicates sharing intent
- "Little Ones" is friendlier than "Kids Menu"

Use Pignal workspaces for major sections and types for item categories within sections.

## Dietary and Allergen Information

### Why This Is Non-Negotiable

Dietary information isn't a nice-to-have — it's a safety and inclusion issue. Failing to disclose allergens can be dangerous. Failing to mark vegetarian/vegan options sends customers away.

### Implementation

- Use consistent symbols or tags: (V) Vegetarian, (VG) Vegan, (GF) Gluten-Free, (DF) Dairy-Free
- Place a legend at the top of the menu
- Note common allergens in descriptions: "contains nuts," "includes dairy"
- Use Pignal tags for dietary categories to enable filtered views

### Seasonal Indicators

Mark seasonal items clearly. This sets expectations (it won't be available forever) and signals freshness (we change our menu with the seasons).

## Common Mistakes

- **Walls of text:** Keep descriptions short. If you need a paragraph, the dish is too complicated to describe — simplify the dish or the description.
- **Inconsistent formatting:** If some items have descriptions and some don't, the bare items look lesser. Be consistent.
- **Outdated prices:** Nothing erodes trust faster than menu prices that don't match the bill. Update immediately.
- **Ignoring mobile:** Most people browse menus on phones before visiting. Descriptions must be scannable on small screens.

## Workflow

1. **Discover** — `get_metadata` for types, workspaces, and limits
2. **Organize** — Plan sections (workspaces) and categories (types)
3. **Write** — `save_item` with sensory descriptions, embedded pricing
4. **Tag** — Dietary tags, seasonal flags, category tags
5. **Review** — Check consistency, pricing accuracy, allergen completeness
6. **Publish** — `validate_item` then `vouch_item`
