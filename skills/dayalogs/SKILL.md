---
name: dayalogs
description: >-
  Create and run surveys with Dayalogs through the Dayalogs MCP server: design
  schema-first questionnaires, validate and import them, preview them for review,
  manage audiences, links, quotas and translations, send email campaigns, and export
  responses. Use whenever "Dayalogs", a dayalogs.com link, or the user's Dayalogs
  surveys, respondents, campaigns or survey results come up.
---

# Dayalogs

Dayalogs is an AI-native survey platform. The `dayalogs` MCP server bundled with this
plugin acts as the signed-in Dayalogs user, inside that user's account. Surveys are
defined as JSON, so the normal loop is: write or edit the definition, validate it,
import it, and preview it before anything reaches a respondent.

## Before you act

1. **Orient first.** Call `get_started` once per session for the current workflow, and
   `survey_authoring_help` (no arguments) for the authoring index before writing or
   editing questions. Use `get_mcp_changes` if a tool you expect seems missing, and
   `get_mcp_backlog` before assuming a capability exists.
2. **Never guess IDs.** Find surveys with `list_surveys`, and read before you write:
   `get_survey_definition` for the whole survey, `get_survey_fragment` for one
   question or block.
3. **Validate before importing.** Call `validate_survey` first, then `import_survey`
   with `mode=create` for a new survey, or `mode=update` with `survey_id` and the
   `expected_version_id` you just read. A version conflict means someone else edited
   the survey: re-read it and tell the user instead of retrying blindly.
4. **Special responses are explicit.** Before authoring "Don't know / No answer" style
   options, read `questions/special-responses/help.md` via `survey_authoring_help`.
   Mark each one with `special.reason`; never infer meaning from codes such as 98/99
   or from the wording.
5. **Privacy notices are the client's.** Never invent or rewrite a privacy notice.
   Keep the approved copy and version, preview it, and get human approval.

## Actions that need an explicit yes

These have real consequences for respondents, costs or data. Do them only when the
user clearly asks, and confirm first:

- `go_live_survey`: starts collecting real responses.
- `send_survey_campaign`: emails real recipients. Always create a draft, run
  `preview_survey_campaign`, and `send_survey_campaign_test` to explicit test
  addresses first.
- `stop_survey_collection`, `archive_survey`: stop collection.
- `delete_survey_response`, `delete_survey_quota`, `delete_tag`,
  `delete_survey_link`, `delete_survey_link_set`, `delete_audience_segment`: deletions.
- `export_survey_responses` with confirmation: exports can be billed. Show the preview
  and its cost first.

## Choosing a tool

| You want to | Use |
|---|---|
| Write a new survey | `survey_authoring_help`, `get_survey_schema`, `validate_survey`, `import_survey` |
| Make a small edit | `get_survey_fragment`, then `patch_survey_fragment`, `append_to_survey` or `append_to_block` |
| Change title, locales, intro or privacy settings | `update_survey_settings` |
| Show the draft to a human | `get_preview_url` (returns a review URL with anchored comments) |
| Check routing and logic | `get_survey_flow` |
| Translate | `get_translation_source`, `validate_survey_translation`, `save_survey_translation` |
| Style and branding | `list_styles`, `get_survey_design`, `update_survey_design` |
| Audiences | `create_audience_import`, `list_audience_contacts`, `create_audience_segment`, tags |
| Distribute | `create_survey_link`, `create_survey_link_set`, `create_survey_embed`, quotas |
| Email campaigns | `create_survey_campaign`, `preview_survey_campaign`, `send_survey_campaign_test` |
| Results | `get_survey_stats`, `get_survey_quality`, `export_survey_responses` |

## Good to know

- Demo sign-ins are isolated sandboxes that start empty and cannot go live.
- Admin preview can skip respondent gating. After publication, check consent screens
  through a controlled public link.
- If a tool returns an error or a conflict, report it plainly. Do not tell the user a
  change was saved unless the tool result confirms it.
