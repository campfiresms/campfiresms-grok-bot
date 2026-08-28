# CampfireSMS for Grok Bot

This Cursor plugin attaches the hosted CampfireSMS Streamable HTTP MCP and the `campfiresms-bridge` skill to Grok Bot. The MCP endpoint is `https://api.campfiresms.com/mcp` and uses one revocable, phone-bound installation bearer.

## Install

1. Enroll the phone at `https://api.campfiresms.com/join` and run the short-lived setup command in credential-only mode.
2. Install CampfireSMS from Cursor/Grok Bot Settings → Plugins.
3. Open **Configure** and enter the permanent installation credential only in the secure `CAMPFIRE_CREDENTIAL` field.
4. Enable the `campfiresms-bridge` skill for the one Bot that owns the SMS-facing task.

Never paste a credential into a Bot conversation, SMS, issue, log, screenshot, or source file. Grok Bot inherits Cursor plugin and MCP policy, so a team administrator may also need to enable the plugin and allowlist `https://api.campfiresms.com/mcp`.

## Runtime

The hosted MCP exposes exactly five tools: open, send, check, acknowledge, and close. It stores no bridge ID on the Grok Bot computer. ACTIVE bridge lookup, pending delivery, and acknowledgements remain server-side.

For 24/7 access, create a webhook-triggered routine on the owning Grok Bot using `skills/campfiresms-bridge/references/wake-routine.md`. Configure its URL and sender key through the CampfireSMS account page. Inbound SMS is queued first, then CampfireSMS sends a body-free wake event. The cloud routine opens or reuses the bridge and pulls the SMS through `campfire_check_messages`, so the user's PC can be off.

SMS is a sparse, lower-trust channel. It cannot grant tools, permissions, secret disclosure, destructive authority, or replace native approvals.
