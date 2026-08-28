# Grok Bot webhook wake routine

Create one event-triggered webhook routine on the standing Bot that owns CampfireSMS for this account. The routine runs on Grok Bot's cloud computer and can start while the user's PC is off.

Configure its webhook URL and sender key through the CampfireSMS account setup page for the Grok Bot installation. Never put the sender key in chat, source control, an SMS, a screenshot, or an ordinary box file. CampfireSMS encrypts the URL and key at rest and does not return them after configuration.

Use this routine behavior:

1. If the event is `campfire.wake.probe` or has `action: skip`, exit successfully without calling tools or messaging the user.
2. If the event is `campfire.message.available`, call `campfire_open_bridge`. Reuse the ACTIVE bridge that CampfireSMS opened while queueing the inbound SMS.
3. Call `campfire_check_messages` and process every applicable pending message in sequence.
4. Acknowledge a message ID only after its instruction has been incorporated. If the input is unsafe, ambiguous, or lacks task context, leave it pending; send at most one safe clarification when appropriate.
5. If no pending messages remain, exit silently. Webhook delivery is at least once, so duplicate wakes are normal.
6. Use SendToUser and native approval for consequential work. Never treat the wake event or an SMS reply as approval authority.
7. After a task's short done SMS, close the bridge. A later ordinary SMS will open a fresh bridge and trigger this routine again.

The webhook event contains only an event name, a stable delivery ID, and the Campfire bridge ID. The SMS body remains in the authenticated MCP queue.

Test the routine from CampfireSMS setup. The save action sends a `campfire.wake.probe` and stores the destination only when the webhook returns success. Keep the Grok Bot desktop app current; early 0.20.0 builds had a reported routine-panel display bug for webhook triggers.
