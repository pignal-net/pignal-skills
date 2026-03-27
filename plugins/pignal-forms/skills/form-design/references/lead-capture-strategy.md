# Lead Capture Strategy

Lead magnets, nurturing sequences, segmentation, and newsletter best practices for converting site visitors into engaged subscribers.

## Lead Magnets

A lead magnet is something valuable you offer in exchange for contact information. The exchange must feel fair -- the visitor gives their email and receives something genuinely useful.

### Effective Lead Magnets for Content Sites

| Lead Magnet | Works Well For | Example |
|-------------|---------------|---------|
| Email newsletter | Sites with regular publishing cadence | "Weekly roundup of new articles" |
| PDF guide / checklist | Sites with educational content | "The Complete Deployment Checklist" |
| Early access | Sites building products or tools | "Be the first to try new features" |
| Exclusive content | Sites with premium insights | "Deep-dive analysis not published on the blog" |
| Community access | Sites with engaged audiences | "Join our private discussion group" |

### Lead Magnet Design Principles

**Promise specific value.** "Get our newsletter" is not a lead magnet. "Get a weekly curated list of the 5 most impactful AI tools, tested by our team" is a lead magnet. The specificity is the value proposition.

**Deliver immediately.** If you promise something in exchange for an email, deliver it instantly. The success message should either contain the value or tell the visitor exactly where to find it ("Check your inbox -- the guide is on its way").

**Match the content context.** A lead magnet on a coding tutorial should offer coding-related value, not a generic newsletter. In Pignal, use inline CTA directives (`{{cta:...}}`) to place context-specific offers within relevant articles.

## CTA-to-Form Connection

The most effective lead capture pattern in Pignal connects a CTA to a form using the `action` parameter instead of a `url`. This keeps the visitor on-page and reduces friction.

### The Pattern

1. Create a newsletter form:
```
create_action: { name: "Newsletter", slug: "newsletter", fields: [{ name: "email", type: "email", label: "Email", required: true }] }
```

2. Configure a post CTA that loads the form inline:
```
update_settings: [
  { key: "cta_post_enabled", value: "true" },
  { key: "cta_post_title", value: "Get weekly insights" },
  { key: "cta_post_description", value: "One email per week. Unsubscribe anytime." },
  { key: "cta_post_button_text", value: "Subscribe" },
  { key: "cta_post_action_slug", value: "newsletter" }
]
```

3. The form appears inline after the button click -- no page navigation, no popup, no friction.

### Combining URL and Action CTAs

Use different CTA slots for different goals:
- **Hero CTA** with a URL -- link to an external signup or product page (high-commitment action)
- **Post CTA** with an action slug -- newsletter signup (low-commitment, high-volume)
- **Sticky CTA** with a URL -- time-sensitive offer or event registration

## Segmentation

Segmentation means grouping subscribers by interest or intent so you can send them relevant content instead of everything.

### Segmentation via Forms

The simplest segmentation strategy uses different forms for different contexts:

- A "Newsletter" form on the general feed captures broad-interest subscribers
- A "React Updates" form embedded via `{{action:react-updates}}` in React-tagged articles captures technology-specific interest
- A "Product Updates" form on the services page captures commercial interest

Each form becomes a segment. Export submissions per form (`export_submissions` with `actionId`) to populate different email lists.

### Segmentation via Select Fields

For a single form that captures different interests:

```
create_action: {
  name: "Subscribe",
  slug: "subscribe",
  fields: [
    { name: "email", type: "email", label: "Email", required: true },
    { name: "interest", type: "select", label: "What interests you most?",
      options: [
        { label: "Technical tutorials", value: "tutorials" },
        { label: "Industry analysis", value: "analysis" },
        { label: "Product updates", value: "product" }
      ]
    }
  ]
}
```

The `interest` field creates segments within a single form. Export and filter by the interest value.

## Newsletter Best Practices

### Frequency

**Consistency beats frequency.** One excellent email per week builds more trust than daily emails of varying quality. Set expectations in the CTA copy: "Weekly insights" or "Monthly deep-dives."

**Match your publishing cadence.** If the site publishes 2-3 times per week, a weekly digest makes sense. If it publishes monthly, a monthly newsletter works. Do not promise a frequency you cannot sustain.

### Content

**Curate, do not blast.** A newsletter is not "everything we published this week." It is the best thing you published, with context about why it matters.

**Add newsletter-only value.** Include a brief commentary, behind-the-scenes note, or upcoming preview that is not on the site. This makes the newsletter worth opening beyond just being a content notification.

### Using Webhooks for Newsletter Automation

Connect form submissions to your email service via webhooks:

1. Set up a webhook endpoint on your email service (Mailchimp, ConvertKit, Resend, etc.)
2. Configure the webhook in Pignal:
```
update_settings: [
  { key: "webhook_url", value: "https://your-email-service.com/api/subscribe" },
  { key: "webhook_events", value: "submission.created" },
  { key: "webhook_secret", value: "your-hmac-secret" }
]
```
3. Your email service receives the submission payload and adds the subscriber automatically.

**Important:** Pignal webhooks fire for ALL form submissions matching the event filter, not per-form. If you have multiple forms and only want newsletter signups sent to your email service, your webhook endpoint needs to filter by `actionName` or `actionId` in the payload.

## Nurturing Strategy

Nurturing means building a relationship over time so that subscribers eventually become customers, collaborators, or advocates.

### The Content Ladder

Structure your content and CTAs as a progression:

1. **Awareness** -- Visitor reads a public article (no form needed, this is traffic)
2. **Interest** -- Visitor subscribes to the newsletter via post CTA (low commitment, single email field)
3. **Consideration** -- Subscriber engages with exclusive content or a guide (higher commitment, may involve a more detailed form)
4. **Action** -- Subscriber reaches out via contact form or purchases (high commitment)

Each step asks for more investment. Pignal's multi-form system supports this naturally -- different forms at different commitment levels.

### Measuring the Funnel

Use Pignal's tools to track each stage:

- **Traffic** -- Page view analytics (built into the site)
- **Subscription** -- `get_submission_stats` for newsletter form submissions
- **Engagement** -- Newsletter open rates and click rates (tracked by your email service)
- **Conversion** -- Contact form or purchase form submissions (`get_submission_stats` for those forms)

Compare conversion rates between stages to identify where visitors drop off. A high traffic / low subscription rate means the CTA is weak or poorly placed. A high subscription / low contact rate means the newsletter is not nurturing effectively.

## Common Lead Capture Patterns

### The Content Upgrade

Offer something related to the article the visitor just read:

```markdown
## Summary

If you found this guide useful, I've put together a one-page checklist version.

{{cta:title="Get the checklist" button="Download" action="checklist-signup" description="One-page PDF. No fluff."}}
```

This works because the visitor has already demonstrated interest in the topic by reading the article. The offer is contextually relevant.

### The Sticky Offer

Use the sticky CTA for high-value, time-limited offers:

```
update_settings: [
  { key: "cta_sticky_enabled", value: "true" },
  { key: "cta_sticky_text", value: "Free workshop this Friday: Building AI-native websites" },
  { key: "cta_sticky_button_text", value: "Reserve Your Spot" },
  { key: "cta_sticky_action_slug", value: "workshop-signup" }
]
```

Disable it after the event by setting `cta_sticky_enabled` to `false`. Do not leave expired offers visible -- it damages credibility.

### The Exit Intent (via Post CTA)

The post CTA appears after the article content, which is naturally where readers who consumed the full article will see it. This functions as a content-aware exit intent:

```
update_settings: [
  { key: "cta_post_enabled", value: "true" },
  { key: "cta_post_title", value: "Before you go..." },
  { key: "cta_post_description", value: "Get articles like this in your inbox every Tuesday" },
  { key: "cta_post_button_text", value: "Subscribe" },
  { key: "cta_post_action_slug", value: "newsletter" }
]
```

This converts the most engaged visitors -- those who read to the end of the article.
