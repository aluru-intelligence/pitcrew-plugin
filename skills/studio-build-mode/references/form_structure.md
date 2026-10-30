---
name: studio-form-structure
description: >
  Reference for studio-forms structural elements: screens, rules, delivery,
  completion, and respondents. Read alongside field_types.md.
---

# Studio Forms — Form Structure Reference

## Screens

A screen is a step in the multi-page form wizard. Fields are grouped into sections.

```json
{
  "id":                   "personal_info",    // unique, snake_case
  "title":               "Your details",
  "estimatedMinutesLeft": 4,                 // shown in progress indicator
  "intro":               "{{member.firstName}} has prefilled this from your records.",
  "submitLabel":         "All correct — continue",
  "summaryOf":           ["full_name", "email"],  // read-only recap at top
  "sections": [
    {
      "title": "Optional section heading",
      "fields": ["full_name", "email", "dob"]
    }
  ]
}
```

Rules:
- A field appears on **at most one** screen.
- Every key in `sections[].fields` must exist in `fields[]`.
- `summaryOf` keys must also exist in `fields[]`.
- Screen order is array order — the client sees "Step N of N".
- Screen `id` values must be unique.

## Cross-field rules

Three kinds of rules go in `rules[]`:

### compare
```json
{
  "kind": "compare", "id": "end_after_start",
  "left": "end_date", "op": "gte", "right": "start_date",
  "message": "End date must be on or after start date."
}
```
Ops: `eq`, `ne`, `lt`, `lte`, `gt`, `gte`.
`right` can be a field key (string) or a literal value.

### sum
```json
{
  "kind": "sum", "id": "allocation_totals_100",
  "fields": ["equity_pct", "bond_pct", "cash_pct"],
  "equals": "100",
  "message": "Allocation must total 100%."
}
```

### expression
```json
{
  "kind": "expression", "id": "holders_different",
  "expr": {"expr": "answers.full_name.first != answers.co_name.first"},
  "message": "The two account holders must be different people."
}
```

## Delivery block

```json
"delivery": {
  "channels":    ["web"],                  // ["web"] or ["web","sms"]
  "from":        "member",                 // who sends the link
  "subject":     "Please complete your annual review",
  "message":     "This should take about 5 minutes.",
  "buttonLabel": "Start your review",
  "verification": {
    "method":         "sms_code",         // or "email_code" or "none"
    "digits":         6,
    "sentTo":         "mobile",           // field key or "primary_mobile"
    "fallbackMethod": "email_code",        // optional
    "fallbackSentTo": "email"             // optional
  },
  "allowFinishLater":          true,
  "remindersOnDays":           [2, 5],
  "remindersBeforeExpiryDays": [1],
  "linkExpires":               {"expr": "answers.needed_by"} // or a date string
}
```

## Completion screen

```json
"completion": {
  "title":          "{{member.firstName}} has everything.",
  "message":        "You'll receive a copy by email.",
  "offerSignedCopy": true,
  "emailSignedCopy": true,
  "variants": [
    {
      "condition": {
        "expr": "respondent == 'client' and respondents.co_client.status != 'submitted'"
      },
      "title": "You've signed. Waiting for the joint holder."
    }
  ]
}
```

`variants[0]` wins when its condition is true; unset keys fall back to the base screen.

## Multi-respondent forms (joint accounts)

Pass `respondents` in `studio_write_fields`. The primary `client` respondent is always
implicit — only declare additional respondents.

```json
"respondents": [
  {
    "role":  "co_client",
    "label": "Joint account holder",
    "presentWhen": {
      "expr": "answers.registration.value in ['jtwros', 'tic']"
    },
    "verification": {
      "method":         "sms_code", "digits": 6, "sentTo": "co_mobile",
      "fallbackMethod": "email_code", "fallbackSentTo": "co_email"
    }
  }
]
```

On each field that belongs to the co-client:
```json
"respondent":  "co_client"
```

On fields visible (read-only) to the other holder:
```json
"sharedWith": ["co_client"]
```

## Placeholders

Available in label and helpText:
- `{{member.firstName}}` — advisor's first name
- `{{org.name}}` — organisation name
- `{{respondent.firstName}}` — current respondent's first name
- `{{progress.prefilled}}` — how many fields were prefilled ("8 of 12 details are already filled in")
- `{{answers.some_key}}` — value of another field (must exist in fields[])

## Connectors

```json
"connectors": [
  {"id": "crm", "kind": "salesforce",
   "label": "Salesforce",
   "options": {"dataModel": "financial_services_cloud"}}
]
```
