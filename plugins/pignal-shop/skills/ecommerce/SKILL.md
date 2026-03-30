---
name: ecommerce
description: |
  E-commerce craft for Pignal Shop sites. Product copywriting, pricing
  psychology, inventory management, collection strategy, and product SEO.
  Use when creating product listings, managing inventory, optimizing shop
  content, or operating a Pignal shop site. Also use for product
  descriptions, pricing tiers, catalog management, or any e-commerce
  content creation.
allowed-tools: [mcp__*]
---

# E-Commerce Craft

Selling products online is fundamentally a writing challenge. Customers can't touch, smell, or try your products — they rely entirely on your descriptions, images, and organization to make purchasing decisions. The craft is in writing copy that creates desire, pricing that feels fair, and organization that makes finding the right product effortless.

## Working with Your Pignal Site

1. Call `get_site_tools` to discover available capabilities
2. Call `get_metadata` to learn content types (product categories), workspaces (collections), and limits

## Product Copywriting

### Benefits Over Features

Features describe the product. Benefits describe what the customer gets from the feature.

- **Feature:** "Made with 304 stainless steel"
- **Benefit:** "Made with 304 stainless steel — won't rust, stain, or retain odors even after years of daily use"

**Why this matters:** Customers don't buy materials — they buy outcomes. The feature is for credibility; the benefit is for desire. Always pair them.

### The Product Description Structure

1. **Headline** (keySummary): Product name + key differentiator
   - "Ceramic Pour-Over Coffee Dripper — Brews Barista-Quality Coffee in 3 Minutes"
2. **Opening benefit**: The single most compelling reason to buy (1 sentence)
3. **Feature-benefit pairs**: 3-5 key features, each linked to a customer benefit
4. **Specifications table**: Technical details for comparison shoppers
5. **Use case**: Who this is for and when they'd use it

### Sensory Language for Physical Products

Since customers can't touch the product, your words must create a tactile experience:

- **Texture:** buttery-soft, rugged, silky-smooth, substantial, lightweight
- **Visual:** hand-finished, polished, matte, translucent, richly colored
- **Quality signals:** precision-machined, hand-stitched, kiln-fired, cold-forged

**When NOT to use sensory language:** Digital products, services, and technical tools. For these, clarity and specificity matter more than atmosphere.

### Specification Tables

Comparison shoppers scan specs, not prose. Use markdown tables for:
- Dimensions, weight, materials
- Compatibility, requirements
- What's included vs what's sold separately

## Pricing Psychology

### Anchoring

The first price a customer sees becomes their reference point. Show the premium option first — it makes the standard option feel affordable by comparison.

### Charm Pricing

- **Value positioning:** $29.95 (the .95 signals "deal")
- **Premium positioning:** $30 (round numbers signal quality and confidence)
- **Choose based on brand:** Discount brands use charm pricing. Luxury brands use round numbers.

### Tier Strategy

If selling multiple versions of a product, use three tiers:
- **Basic:** Stripped down, entry-level price (draws people in)
- **Standard:** The one you actually want to sell (most features, best value)
- **Premium:** All features, highest price (makes Standard look reasonable)

### Price Framing

Frame prices in terms of value:
- "Less than a coffee a day" (subscription framing)
- "Replaces $200 of disposable alternatives per year" (ROI framing)
- "Free shipping on orders over $50" (threshold incentive)

## Inventory Status Communication

Be honest and specific about availability:
- **In Stock** — "Ships within 1-2 business days"
- **Low Stock** — "Only 3 remaining" (creates urgency without dishonesty)
- **Out of Stock** — "Back in stock [date]" or "Join the waitlist"
- **Discontinued** — "While supplies last" or "See [replacement product]"

**Why honesty matters:** Selling a product that's actually out of stock destroys trust permanently. Better to show "out of stock" than to accept an order you can't fulfill.

## Collection Strategy

Collections (Pignal workspaces) are how customers browse your shop. Design them around how customers shop, not how you organize inventory:

- **By use case:** "Gift Ideas," "Everyday Carry," "Home Office"
- **By audience:** "For Developers," "For Designers," "For Teams"
- **By occasion:** "New Arrivals," "Best Sellers," "On Sale"

**Why this matters:** A customer browsing "Gift Ideas Under $50" has clear intent. A customer browsing "Category: Electronics > Subcategory: Accessories" is doing work you should do for them.

## Product SEO

### Title Optimization

Product titles should include:
- Brand name (if recognized)
- Product type (what it is)
- Key differentiator (size, color, material, edition)
- "50mm f/1.4 Lens for Canon EF Mount" not just "Camera Lens"

### Tag Strategy

Tags in a shop context drive filtered browsing:
- Material tags: "leather," "stainless-steel," "organic-cotton"
- Use-case tags: "travel," "office," "outdoor"
- Attribute tags: "handmade," "eco-friendly," "limited-edition"

## Common Mistakes

- **Features without benefits:** Listing specs that mean nothing to the customer. "Bluetooth 5.0" → "Bluetooth 5.0 — stable connection up to 30 feet, even through walls"
- **Identical descriptions:** Copy-pasting the same description for product variants. Each variant should highlight what's different about IT.
- **No social proof:** Include customer quotes, ratings, or "most popular" badges when possible.
- **Wall of text:** Product descriptions should be scannable. Use bullets, bold key points, and keep paragraphs short.

## Workflow

1. **Discover** — `get_metadata` for types (product categories), workspaces (collections), limits
2. **Structure** — Plan collections and categories
3. **Write** — `save_item` with benefit-driven copy, spec tables
4. **Tag** — Material, use-case, and attribute tags
5. **Price** — Apply pricing psychology appropriate to brand positioning
6. **Publish** — `validate_item` then `vouch_item`

For deeper guidance, see [product-copywriting.md](references/product-copywriting.md) and [pricing-display.md](references/pricing-display.md).
