---
name: studio-build-mode
description: >
  Step-by-step procedure for building a studio-forms JSON artifact using
  the studio_* MCP tools. Use when the user asks to build a form, intake,
  questionnaire, or any client-facing document. Covers all 5 tools in order,
  field type selection, and error recovery.
compatibility: Studio Builder
---

## Role

You build studio-forms JSON artifacts by calling MCP tools.
You write the complete `fields[]` array in one call and the `screens[]` in one call.
You do NOT stream or build incrementally field-by-field — write the full arrays at once.

## Tools at your disposal

| Tool | When |
|---|---|
| `studio_create_session` | First call — creates the session, returns `session_id` |
| `studio_get_form` | Before any write on an existing draft; after each write to verify |
| `studio_write_fields` | Write form header + ALL fields atomically |
| `studio_write_structure` | Write ALL screens + rules + delivery updates + completion |
| `studio_validate_form` | After EVERY write — fix all errors before continuing |
| `studio_list_sessions` | To find an existing session to resume |

## Steps

### 1. Understand the form

Before calling any tool:
- Identify the form's **purpose**, **audience**, and **channel** (web, SMS, or both).
- List every field needed: key, type, label, required/optional, prefill source.
- Group fields into logical **screens** (2–4 fields per screen is ideal).
- Identify any **conditional fields** (`visibleWhen`), **rules**, and **multi-respondent** needs.
- Search the user's **document collections** if they mention existing forms or field lists.

If anything is unclear, ask ONE focused question before building.

### 2. Plan (show to user, wait for approval)

Show the plan clearly:

```
**Form:** Annual Client Review
**Fields (6):**
  • full_name (person_name) — prefilled from CRM
  • email (email) — prefilled from CRM
  • dob (date_of_birth) — required, minAge 18
  • risk (single_select) — conservative / moderate / aggressive
  • goals (short_text) — optional
  • consent_ack (consent) — v1

**Screens (2):**
  1. Your details — full_name, email, dob
  2. Annual check-in — risk, goals, consent_ack

**Delivery:** SMS verification, allow finish later

Does this look right? Reply "yes" to build it.
```

### 3. Create session

```
studio_create_session(title="Annual Client Review")
→ save the returned session_id for all subsequent calls
```

### 4. Write fields

Read `skills/studio-build-mode/references/field_types.md` to pick the exact type strings.

Call `studio_write_fields` ONCE with:
- `form_header`: title, schemaVersion "0.3.0", delivery block
- `fields`: the complete desired fields[] array

After the call, call `studio_validate_form` and fix any errors.

### 5. Write structure

Call `studio_write_structure` ONCE with:
- `screens`: complete desired screens[] — every field key must exist in fields[]
- `rules`: cross-field rules (if any)
- `completion`: title, message, offerSignedCopy (optional)

After the call, call `studio_validate_form` and fix any errors.

### 6. Show the result

Call `studio_get_form` and show the returned `form_draft` to the user in a fenced code block.

```json
{
  "schemaVersion": "0.3.0",
  "title": "Annual Client Review",
  ...
}
```

Then say:
> "Form built ✓ — **{title}** has {N} fields across {S} screens, schema 0.3.0.
> You can copy this JSON and import it, or ask me to make any changes."

## Error recovery

If `studio_validate_form` returns errors:
- For "Screen 'X' references unknown field key 'Y'" → rewrite screens without that key, or add the field via `studio_write_fields`
- For "Missing required top-level key: 'title'" → re-call `studio_write_fields` with `form_header.title` set
- For "Form title is empty" → same as above

Max 3 repair attempts; if still failing, tell the user which errors remain.

## Key rules

- NEVER invent type names. Use only canonical strings from field_types.md.
- `signature` needs `config.signers[{role, nameField}]` and `config.typedMustMatch`.
- `consent` needs `config.text` and `config.textVersion`.
- `government_id` and `bank_account` are **secret and web-only** — never include them in SMS forms.
- Conditional fields: `visibleWhen` and `requiredWhen` are always `{"expr": "..."}` objects.
- Binding: `{"source": "system_of_record", "path": "client.name", "confidence": "high"}`
- Multi-respondent: pass `respondents` in `studio_write_fields`; add `"respondent": "co_client"` on co-client fields.

## References

- `skills/studio-build-mode/references/field_types.md` — all 52 types with examples
- `skills/studio-build-mode/references/form_structure.md` — screens, rules, delivery, completion
