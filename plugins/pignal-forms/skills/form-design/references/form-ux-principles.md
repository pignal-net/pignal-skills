# Form UX Principles

Field psychology, error handling, accessibility, and mobile-first design for forms that convert.

## Field Psychology

### Cognitive Load

Every form field imposes a decision cost on the visitor. The visitor must understand what is being asked, decide what to answer, and type or select a response. This cognitive load accumulates across fields.

**The 3-field rule:** Forms with 3 or fewer fields consistently outperform forms with more. Each additional field beyond 3 creates measurable friction. If you need more than 3 fields, you need a strong reason for each one.

**The disclosure pattern:** Start with the minimum (email for a newsletter, email + message for contact), and reveal additional fields only after the visitor has committed to the core interaction. In Pignal, this means designing your primary forms lean and creating separate, more detailed forms for deeper engagement.

### Field Ordering

Order matters. Place fields in this sequence:

1. **The easiest field first.** Name or email -- something the visitor can answer without thinking. This creates momentum.
2. **Selection fields before text fields.** Choosing from a dropdown is easier than composing text. It keeps momentum going.
3. **The hardest field last.** Textarea for messages or long-form input. By this point, the visitor has invested effort and is more likely to complete.

### Labels and Placeholders

**Labels above fields, not inside them.** Placeholder text that disappears when the visitor starts typing forces them to rely on memory. A persistent label above the field is always visible.

**Labels should be questions or instructions, not database column names.**
- "What should we call you?" beats "Name"
- "Where can we reach you?" beats "Email"
- "Tell us what you need" beats "Message"

**Placeholder text is for examples, not instructions.** Use it to show format: "jane@example.com" for email, "+1 (555) 123-4567" for phone.

## Error Handling

### Inline Validation

Pignal's `FieldTypeRegistry` validates each field on submission. The validation is server-side -- there is no client-side JavaScript validation. This means errors appear after the visitor submits the form.

**Clear error messages matter more than instant feedback.** When validation fails, the error message should:
- Name the specific field that has the problem
- Explain what is wrong in plain language
- Suggest how to fix it

Pignal's built-in messages follow this pattern: "{Label} must be a valid email address", "{Label} must be at most {maxLength} characters", "{Label} must be one of: {options}".

### Success Feedback

The `successMessage` in form settings is what the visitor sees after successful submission. Make it:
- **Specific about what happens next.** "We'll respond within 24 hours" beats "Thanks!"
- **Reassuring.** "Your message was sent successfully" confirms the action worked.
- **Actionable when appropriate.** "Check your inbox for a confirmation email" gives a next step.

### Graceful Degradation

If a form referenced by `{{action:slug}}` does not exist or is inactive, Pignal renders nothing in its place. The page does not break -- the directive is silently omitted. This means:
- You can safely reference forms in content before creating them (the content will work, just without the form)
- Deactivating a form does not break pages that reference it
- You should monitor for "orphaned" directives -- forms referenced in content that no longer exist

## Accessibility

### Semantic Structure

Pignal's `action-form` component renders forms with semantic HTML:
- Each field has a `<label>` element properly associated with its input
- Required fields are marked in the HTML
- Error messages are associated with their fields
- The submit button uses `<button type="submit">`

### Keyboard Navigation

All Pignal form fields are standard HTML inputs and selects. They support:
- Tab to move between fields
- Enter to submit the form
- Arrow keys for select dropdowns

### Screen Reader Considerations

When designing forms:
- Use descriptive labels that make sense when read aloud in sequence
- Set meaningful `description` text on the form itself -- it provides context before the fields
- Use `successMessage` that communicates clearly without visual cues
- Avoid relying on color alone to communicate state (Pignal's error messages use text, not just color)

## Mobile-First Design

### Touch Targets

Pignal's form inputs are rendered at standard heights that meet the 44px minimum touch target guideline. When designing forms:
- Avoid cramming many fields into a small space
- Select fields are particularly mobile-friendly because they trigger the native picker
- Textarea fields should have reasonable max-length values to prevent extremely long inputs on mobile

### Field Type Selection for Mobile

Choose field types that trigger the right mobile keyboard:
- `email` triggers the email keyboard (with @ key)
- `tel` triggers the phone keypad
- `url` triggers the URL keyboard (with .com key)
- `number` triggers the numeric keypad
- `text` triggers the standard keyboard

This is a free usability improvement -- the right field type means the visitor does not have to switch keyboards.

### Vertical Layout

Mobile forms are single-column by default in Pignal. This is correct. Multi-column form layouts on mobile create confusion about reading order and make touch targets too small.

## Form Field Selection Guide

### When to Use Each Field Type

| Goal | Field Type | Why |
|------|-----------|-----|
| Collect a name | `text` with maxLength 100 | Short, predictable input |
| Collect an email | `email` | Built-in format validation, correct mobile keyboard |
| Collect a message | `textarea` with maxLength 2000-5000 | Allows multi-line input for detailed responses |
| Offer predefined choices | `select` with options | Constrains input, reduces ambiguity, mobile-friendly |
| Collect a website | `url` | Validates URL format, correct mobile keyboard |
| Collect a phone number | `tel` | Flexible format validation, correct mobile keyboard |
| Collect a quantity or rating | `number` | Validates numeric input, numeric keyboard |
| Get explicit consent | `checkbox` | Boolean yes/no, required for legal agreements |

### When NOT to Use a Field

- **Do not add a phone field unless you will actually call.** Asking for a phone number and never using it erodes trust.
- **Do not add a "how did you hear about us" field.** It adds friction and the data is unreliable. Use analytics instead.
- **Do not add optional fields "just in case."** If you are not sure you need it, you do not need it. You can always add fields later via `update_action`.
- **Do not use `text` when `select` works.** If there are fewer than 10 valid answers, use select. It prevents typos and makes submissions easier to analyze.

## Progressive Enhancement

### Start Simple, Add Later

Design forms for the immediate need, not the eventual need:

1. **Week 1:** Simple contact form (name, email, message)
2. **After 20 submissions:** Review what visitors actually ask about. Add a select field to categorize inquiries if a pattern emerges.
3. **After 100 submissions:** Consider splitting into separate forms for different purposes (support vs. sales vs. general)
4. **Ongoing:** Use `export_submissions` to analyze patterns and refine fields

This approach means you never over-engineer the form. Each change is based on real submission data, not assumptions.
