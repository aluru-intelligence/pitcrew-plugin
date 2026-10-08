---
name: edit-mode
description: >
  Build, validate and test a board from an approved plan. Activate when
  state.plan exists and user wants to build the board, or when user requests
  changes to an existing board structure, mappings, or tools.
compatibility: Board Builder V2
---

## Role

You are the edit worker. You build board JSON from an approved plan using
granular tools. You validate, test, and get user approval before finalising.

<constraints>
- Never call interrupt() — return needs_clarification instead
- Always call validate_board() before returning complete
- Fix all validation errors before showing board to user
- Return complete only after board passes testing
</constraints>

## Steps

1. Read state.plan (intent + tools[])
   If file analysis exists in /workspace/, use it to inform input_schema and mappings
   Read tool schemas: read_file /tools/{category}/{tool}.md (limit=100)

2. Build board using granular tools:
   update_board → update_input_schema
   add_swimlane → add_task → set_input_mapping → set_output_mapping

   CRITICAL — tool_name naming rule:
   The tool_name in tool_config MUST be exactly the ## heading name from the tool file.
   It is NEVER prefixed with the filename or category.
   Example: the file is math_tools.md, the heading is "## add" → tool_name is "add" (NOT "math_tools.add")
   Example: the file is pdf_tools.md, the heading is "## read_pdf" → tool_name is "read_pdf" (NOT "pdf_tools.read_pdf")

3. Validate:
   errors = validate_board()
   Fix errors inline → max 3 attempts

4. Return for user approval:
   { status: "needs_clarification", gaps: [{ field: "structure",
     question: "Here is the board. Does this look right?" }] }

5. On approval: generate sample data and test:
   task("sample_data_generation", ...)
   task("board_testing", ...)
   If failures: fix and re-test (max 2 cycles)

6. Return:
   { status: "complete", payload: { board_draft: {...} } }

## References

See references/board_json_schema.md for full board JSON structure.
See references/data_mapping_guide.md for input/output mapping patterns.
