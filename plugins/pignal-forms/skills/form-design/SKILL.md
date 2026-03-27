# Form Design and Lead Capture

Form design and lead capture for any Pignal site. Form UX best practices, conversion optimization, CTA strategy, and webhook integration. Use when creating forms, adding contact forms, setting up newsletter signups, managing submissions, building CTAs, or working with site actions.

---

## The MCP Workflow

Every form operation follows this sequence. Do not skip steps.

1. **Discover** -- `get_site_tools` to learn what the site can do.
2. **Learn the site** -- `call_site_tool` with `get_metadata` to get types, workspaces, and current settings. Check for existing CTA settings and form conventions.
3. **Plan** -- Decide the form's purpose, fields, and placement strategy before creating anything.
4. **Create** -- `call_site_tool` with `create_action` to build the form.
5. **Embed** -- Place the form using content directives (`{{action:slug}}`) or configure CTA settings to display it across the site.
6. **Monitor** -- Track submissions via `list_submissions` and `get_submission_stats`.
7. **Integrate** -- Set up webhooks to push submissions to external systems.

---

## Form UX Principles

### Minimize Fields

Every field you add reduces completion rate. The question is not "what would be nice to know?" but "what is the minimum needed to fulfill the form's promise?"

**Contact form:** name, email, message. Three fields. Not company, phone, job title, how-did-you-find-us, preferred-contact-method. Those questions can come later in the conversation.

**Newsletter signup:** email. One field. Not name, company, interests, frequency preference. You can segment later via behavior.

**Feedback form:** message. One field, maybe two with email for follow-up. Not name, topic, urgency, satisfaction rating.

### Clear Labels

Labels should tell the visitor exactly what to enter, not what the field is called in the database.

- "Your email address" beats "Email"
- "What can we help with?" beats "Message"
- "Company website" beats "URL"

### Smart Defaults

Use the right field type for the data you need. Pignal supports 8 field types, each with built-in validation:

| Field Type | Use For | Validation |
|------------|---------|------------|
| `text` | Short text (names, subjects) | Max length |
| `email` | Email addresses | Format check (contains @ and .) |
| `textarea` | Long text (messages, feedback) | Max length |
| `select` | Choice from predefined options | Must match option values |
| `url` | Website addresses | Must start with http:// or https:// |
| `tel` | Phone numbers | Digits, spaces, +, -, parentheses |
| `number` | Numeric values (quantities, ratings) | Must be a valid number |
| `checkbox` | Yes/no consent, agreements | Boolean (true/false) |

### Required vs Optional

Mark only truly required fields as `required: true`. A form where every field is required feels like an interrogation. A form with two required fields and three optional ones feels like a conversation.

**Always required:** The primary contact method (usually email) and the core ask (usually message or selection).

**Usually optional:** Name, phone, company, additional details.

---

## Creating Forms via MCP

### Basic Contact Form

```
call_site_tool(id, "create_action", {
  name: "Contact Form",
  slug: "contact",
  description: "Get in touch with us",
  fields: [
    { name: "name", type: "text", label: "Your name", required: true, maxLength: 100 },
    { name: "email", type: "email", label: "Email address", required: true },
    { name: "message", type: "textarea", label: "How can we help?", required: true, maxLength: 2000 }
  ],
  settings: {
    successMessage: "Thanks for reaching out! We'll get back to you soon.",
    submitLabel: "Send Message"
  }
})
```

### Newsletter Signup

```
call_site_tool(id, "create_action", {
  name: "Newsletter",
  slug: "newsletter",
  description: "Stay updated with our latest posts",
  fields: [
    { name: "email", type: "email", label: "Email address", required: true }
  ],
  settings: {
    successMessage: "You're subscribed! Check your inbox.",
    submitLabel: "Subscribe"
  }
})
```

### Feedback Form with Options

```
call_site_tool(id, "create_action", {
  name: "Feedback",
  slug: "feedback",
  fields: [
    { name: "type", type: "select", label: "What kind of feedback?", required: true,
      options: [
        { label: "Bug report", value: "bug" },
        { label: "Feature request", value: "feature" },
        { label: "General feedback", value: "general" }
      ]
    },
    { name: "message", type: "textarea", label: "Tell us more", required: true, maxLength: 3000 },
    { name: "email", type: "email", label: "Email (optional, for follow-up)" }
  ],
  settings: {
    successMessage: "Thanks for your feedback!",
    submitLabel: "Submit Feedback"
  }
})
```

---

## Content Directives

Pignal uses content directives to embed interactive elements inside markdown content. Directives are processed server-side during rendering -- they work without client-side JavaScript (except HTMX for form loading).

### The `{{action:slug}}` Directive

Embeds a form inline within any item's content. The form loads via HTMX when the page renders.

**Usage in markdown:**

```markdown
## Get in Touch

Have a question about this article? Drop us a message.

{{action:contact}}
```

The directive renders the form defined by the `contact` action slug. If the action does not exist or is not active, the directive renders nothing (graceful degradation).

### The `{{cta:...}}` Directive

Renders an inline CTA card with a heading, optional description, and a button that either links to a URL or loads a form.

**Syntax:**

```
{{cta:title="Ready to get started?" button="Sign Up" url="https://example.com/signup"}}
```

```
{{cta:title="Join our newsletter" button="Subscribe" action="newsletter" description="Weekly insights delivered to your inbox."}}
```

**Parameters:**
- `title` (required) -- CTA heading text
- `button` (required) -- Button label
- `url` -- External link (opens in new tab)
- `action` -- Action slug to load a form inline (mutually exclusive with `url`)
- `description` -- Optional subtext below the heading

When `action` is provided, the button triggers an HTMX request to load the form inline. When `url` is provided, the button becomes a standard link.

### Other Useful Directives

- `{{callout type="info" text="Important note"}}` -- Colored callout boxes (info, warning, success, error, tip)
- `{{button text="Get Started" url="/form/contact" variant="primary"}}` -- Standalone button links
- `{{testimonials}}` or `{{testimonials limit="3"}}` -- Testimonial card grid
- `{{latest type="Article" limit="3"}}` -- Recent items feed
- `{{gallery tag="photos" columns="3"}}` -- Image/item grid

---

## CTA Strategy

CTAs (calls to action) convert passive readers into engaged leads. Pignal supports four placement strategies through site settings, plus inline placement via directives.

### Settings-Based CTAs

These are configured once via `update_settings` and automatically appear across the site. No per-item configuration needed.

#### Hero CTA

Full-width banner at the top of the source page (homepage). Use for the site's primary conversion goal.

```
call_site_tool(id, "update_settings", {
  settings: [
    { key: "cta_hero_enabled", value: "true" },
    { key: "cta_hero_title", value: "Build your AI-powered website" },
    { key: "cta_hero_description", value: "Create, manage, and deploy through AI agents" },
    { key: "cta_hero_button_text", value: "Get Started" },
    { key: "cta_hero_button_url", value: "https://example.com/signup" }
  ]
})
```

To connect a hero CTA to a form instead of a URL:

```
{ key: "cta_hero_action_slug", value: "newsletter" }
```

#### Post CTA

Card shown after article content on item pages. Use for contextual offers related to what the reader just consumed.

```
call_site_tool(id, "update_settings", {
  settings: [
    { key: "cta_post_enabled", value: "true" },
    { key: "cta_post_title", value: "Enjoyed this article?" },
    { key: "cta_post_description", value: "Get notified when we publish new content" },
    { key: "cta_post_button_text", value: "Subscribe" },
    { key: "cta_post_action_slug", value: "newsletter" }
  ]
})
```

#### Sticky CTA

Fixed bottom bar that persists while scrolling. Use for urgent or time-sensitive offers. Includes a dismiss button.

```
call_site_tool(id, "update_settings", {
  settings: [
    { key: "cta_sticky_enabled", value: "true" },
    { key: "cta_sticky_text", value: "Limited spots available for our next cohort" },
    { key: "cta_sticky_button_text", value: "Reserve Your Spot" },
    { key: "cta_sticky_button_url", value: "https://example.com/enroll" }
  ]
})
```

### CTA Placement Strategy

Not every page needs every CTA. The goal is to present the right offer at the right moment.

**Hero CTA** -- Best for the homepage. Readers arriving at the source page are exploring -- give them a clear next step. High visibility, moderate conversion rate.

**Post CTA** -- Best for item pages. Readers who finish an article are engaged and primed for deeper commitment. Lower visibility (only post readers), higher conversion rate.

**Sticky CTA** -- Best for time-sensitive offers or high-priority conversions. Use sparingly -- a persistent bar can feel pushy if the offer is not compelling. Include the dismiss button (Pignal does this automatically).

**Inline CTA** (via `{{cta:...}}` directive) -- Best for contextual, content-specific offers. Place them where the content naturally creates a desire to act. Example: after a section about a specific feature, add a CTA that lets the reader try that feature.

### CTA Copy Principles

**Button text should name the action, not describe it.**
- "Subscribe" beats "Click to subscribe"
- "Get Started" beats "Learn more about getting started"
- "Download the Guide" beats "Submit"

**Headlines should state the value, not the mechanism.**
- "Stay ahead of the curve" beats "Sign up for our newsletter"
- "Ship your site in 60 seconds" beats "Create an account"

**Descriptions should handle objections.**
- "Free forever. No credit card required." handles the cost objection
- "One email per week. Unsubscribe anytime." handles the spam objection
- "Join 500+ developers" handles the credibility objection

---

## Webhook Integration

Webhooks push form submissions to external systems in real-time. Pignal fires HTTP POST requests with HMAC-SHA256 signed payloads.

### Setting Up Webhooks

```
call_site_tool(id, "update_settings", {
  settings: [
    { key: "webhook_url", value: "https://your-service.com/webhook" },
    { key: "webhook_events", value: "submission.created" },
    { key: "webhook_secret", value: "your-secret-key-here" }
  ]
})
```

### Webhook Payload

When a form is submitted, Pignal sends:

```json
{
  "event": "submission.created",
  "timestamp": "2025-01-15T10:30:00Z",
  "data": {
    "actionId": "...",
    "actionName": "Contact Form",
    "fields": { "name": "Jane", "email": "jane@example.com", "message": "Hello!" }
  }
}
```

### HMAC-SHA256 Verification

If `webhook_secret` is configured, every request includes an `X-Pignal-Signature` header:

```
X-Pignal-Signature: sha256=<hex-encoded-hmac>
```

The signature is computed over the raw JSON body using the secret as the HMAC key. Your receiving endpoint should:

1. Read the raw request body
2. Compute `HMAC-SHA256(body, secret)` and hex-encode it
3. Compare with the value after `sha256=` in the header
4. Reject the request if they do not match

### Webhook Events

| Event | Fires When |
|-------|-----------|
| `submission.created` | A visitor submits a form |
| `item.published` | An item is vouched to public visibility |

Set `webhook_events` to a comma-separated list of events to listen for. If empty or unset, all events fire.

### Delivery Model

Webhooks are fire-and-forget. Pignal sends the request and does not retry on failure. This is a deliberate design choice -- webhook delivery is best-effort. For critical integrations, use the `list_submissions` and `export_submissions` tools as a reliable backup.

---

## Managing Submissions

### Viewing Submissions

```
call_site_tool(id, "list_submissions", { actionId: "...", limit: 20 })
```

Filter by action to see submissions for a specific form. Use `status` to filter by submission state.

### Submission Lifecycle

Submissions start as `new`. Move them through states to track processing:

```
call_site_tool(id, "manage_submission", { submissionId: "...", status: "reviewed" })
```

### Exporting Data

Export all submissions for an action as JSON or CSV:

```
call_site_tool(id, "export_submissions", { actionId: "...", format: "csv" })
```

CSV export is useful for importing into spreadsheets, CRMs, or email marketing tools.

### Submission Statistics

Get aggregate counts per action:

```
call_site_tool(id, "get_submission_stats")
```

Returns total submission counts grouped by action. Use this to compare form performance across the site.

---

## Form Analytics and Optimization

### Measuring Form Performance

Track these metrics to understand whether forms are working:

- **Submission count** -- Total submissions per form over time. Is the form getting used?
- **Completion rate** -- Submissions / page views on pages containing the form. Are visitors who see the form actually completing it?
- **Field abandonment** -- If submissions are low despite traffic, the form may have too many fields or confusing labels.

### Optimization Cycle

1. **Baseline** -- Check `get_submission_stats` to establish current performance.
2. **Hypothesize** -- Identify the likely bottleneck (too many fields, unclear value proposition, poor placement).
3. **Change one thing** -- Reduce fields, rewrite CTA copy, move form placement. Change one variable at a time.
4. **Measure** -- Wait for sufficient submissions (at least 20-30) to compare.
5. **Iterate** -- Keep what works, test the next hypothesis.

### Common Fixes

| Symptom | Likely Cause | Fix |
|---------|-------------|-----|
| Low submission count, high traffic | Weak CTA copy or poor placement | Rewrite headline/button, move form above the fold |
| Submissions but wrong audience | Form attracts unqualified leads | Add a qualifying field (select with options) |
| Partial submissions | Too many required fields | Make non-essential fields optional |
| Spam submissions | No validation barriers | Add a required checkbox ("I'm a real person") |

---

## References

For deeper exploration of specific topics:

- **[Form UX Principles](references/form-ux-principles.md)** -- Field psychology, error handling, accessibility, and mobile-first form design.
- **[Lead Capture Strategy](references/lead-capture-strategy.md)** -- Lead magnets, nurturing sequences, segmentation, and newsletter best practices.
