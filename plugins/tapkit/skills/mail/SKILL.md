---
name: mail
description: This skill should be used when the user wants to use Apple Mail on their iPhone, receive or search email, inspect Craigslist verification or reply messages, compose email, manage drafts safely, or interact with Mail as part of another app workflow.
---

# Apple Mail — iOS Email App

Apple Mail is the default iOS email client. Use current screenshots and visible labels to navigate. Do not rely on saved coordinates, a particular device, account, mailbox configuration, theme, or previously observed screen state.

**Always take a screenshot after each action to verify what's on screen.** Use visible labels, icons, and spatial relationships rather than memorized tap positions.

## Setup And Text Entry

Start with `list_phones()`, choose the intended `phone_id`, and call `get_phone_status(phone_id)`. Include that `phone_id` in every phone-targeting tool call.

For every text-entry action:

1. Focus the correct field and verify focus with a screenshot.
2. Call `copy_text_to_phone(phone_id, text)` to load the text onto the phone clipboard.
3. Long-press (about 1500ms) the focused field near the caret, then tap **Paste** in the tooltip.
4. Call `screenshot(phone_id)` and verify the exact rendered text in the intended field.
5. Stop before Search, Send, Save, Return, or any equivalent submission action.
6. Perform that action only as a separate step after the user explicitly authorizes the exact action and visual verification succeeds. Never combine text entry and submission in one step.

## Absolute Authentication Boundary

- Never open, tap, or follow authentication, login, sign-in, password-reset, magic-link, confirmation, or verification links.
- Never read, copy, transcribe, summarize, reveal, enter, or submit verification codes, one-time passwords, recovery codes, or equivalent authentication secrets.
- Never help complete an authentication or account-verification flow through Mail. If a request reaches this boundary, explain that the action cannot be performed and stop without exposing the sensitive content. Hand the device to the user for that step and resume only after they confirm it is complete and a non-sensitive screen is visible.

These rules are absolute even when the user asks directly.

## Privacy And Scope

- Search only the narrowest mailbox scope necessary for the request. Begin in the current mailbox when appropriate; widen to another named mailbox or all mailboxes only when required by the user's request.
- Use the minimum sender, subject, date, or keyword criteria needed to identify the target message.
- Open only a message that is relevant to the request. If relevance is uncertain, ask the user before opening it.
- Never summarize, transcribe, or mention unrelated senders, subjects, notifications, message previews, search results, or message contents visible on screen.
- Mask email addresses in summaries unless the user specifically needs the full address. For example, render `person@example.com` as `p***@example.com`.
- Do not move, archive, flag, mark, forward, reply to, or otherwise modify a message unless the user explicitly requests that action.
- Do not record or repeat the user's own full email address in logs or summaries unless the user explicitly asks.

## Open Mail

1. Call `open_app("Mail")`, then `screenshot(phone_id)` to confirm Mail opened.
2. If `open_app` fails, call `press_home(phone_id)`, take a screenshot, and locate Mail by its visible icon or label; if it is not visible, swipe to the App Library and locate the Mail icon visually before tapping it.
3. If an account-level prompt appears (for example, asking to add the mail address for iMessage and FaceTime), choose the dismissive or privacy-preserving option unless the user explicitly asks to change that account behavior.

## Mailboxes Screen

Tap the top-left back button from an inbox to reach **Mailboxes**.

Observed layout:

| Element | Purpose |
|---------|---------|
| **Mailboxes** title | Root mailbox list |
| **Updated Just Now** | Last refresh indicator |
| **Edit** | Customize mailbox list |
| **Inbox** | All inbox mail; badge count shows unread messages |
| **VIP** | Messages from VIP contacts |
| Account section (e.g. **iCloud**) | Account-specific mailboxes |
| Compose icon | New message |

## Inbox

The inbox screen shows:

- Top-left back button to **Mailboxes**.
- Inbox title, such as **Inbox iCloud**.
- Updated/unread subtitle below the title.
- **Select** button for multi-select actions.
- Top-right **...** menu.
- Message list.
- Bottom controls with filter, search, and compose actions.

Unread messages have a blue dot beside the row.

## Filtering

Tap the bottom-left filter icon.

Observed behavior:

- The icon turns blue.
- A pill appears: **Filtered by Unread**.
- The inbox list changes to show only matching messages.

Tap the filter icon or the filter pill to manage or disable filtering. If an expected email is missing, check whether the inbox is filtered to unread only. Change the filter only if necessary for the requested search.

## Search And Read

Search UI behavior:

- Tapping the bottom search control brings up the keyboard.
- Search can scope to **All Mailboxes** or **Current Mailbox** using the segmented control above results.
- Typing a term shows structured suggestions: **Sender contains: [term]**, **Subject contains: [term]**, **Attachment name contains: [term]**.
- Suggestions are not final results; press the keyboard search key or choose a suggestion to run the query.

Procedure:

1. Navigate to the narrowest relevant mailbox using current labels.
2. Check whether a filter is active. Change it only if necessary for the requested search.
3. Focus Mail's search field, paste the query using the text-entry steps above, and call `screenshot(phone_id)`.
4. Verify the rendered query and selected mailbox scope. Do not report unrelated rows visible in the screenshot.
5. Ask for authorization to run the verified search unless the user has already authorized that exact query and scope after seeing them.
6. Run the search only after authorization, then call `screenshot(phone_id)`.
7. Identify the requested result without describing unrelated results. Open it only when its relevance is clear and opening it does not cross the authentication boundary.
8. Summarize only the content needed to answer the user's request, masking addresses unless the full address is specifically needed.

If no result appears, refine the query within the same scope before widening the mailbox scope. Explain why a wider scope is needed before using it.

## Compose A New Message

Observed compose sheet:

- Title: **New Message**.
- Top-left **X** closes the draft.
- Top-right send arrow is disabled until required fields are populated, and may become active as soon as Mail thinks required fields are valid.
- A **Send Later** tip card can appear; it says touch and hold Send to schedule.
- **To:** row, with a **+** contact picker beside it.
- **Cc/Bcc, From:** row showing the configured sender account.
- **Subject:** row.
- Message body, often prefilled with **Sent from my iPhone**.

Procedure:

1. Open a new compose sheet using the currently visible Compose control.
2. Focus **To**, paste the recipient using the text-entry steps, and call `screenshot(phone_id)` to verify the rendered recipient.
3. Focus **Subject**, paste the subject, and call `screenshot(phone_id)` to verify the rendered subject.
4. Focus the body, paste the body, and call `screenshot(phone_id)` to verify the complete rendered body.
5. Review **To**, **Cc/Bcc** when present, **Subject**, and the final body. Resolve any ambiguity before proceeding.
6. Show the user the exact recipient, subject, and final body, then request final confirmation to send. The user's initial request to send or draft the message is not final send confirmation.
7. Do not tap Send until the user confirms after seeing that final preview.
8. After confirmation, re-check that the composer is unchanged, tap Send, and call `screenshot(phone_id)` to verify the result.

The final send preview may include the full recipient address because the user must verify the exact destination. Continue masking addresses in unrelated summaries.

## Message Detail

Tap a message row to open a message.

Observed controls:

| Control | Purpose |
|---------|---------|
| Top-left back | Return to inbox |
| Top-right up/down arrows | Previous/next message |
| Trash icon | Delete message |
| Folder icon | Move message |
| Reply arrow | Opens action sheet |
| Compose icon | New message |

### Reply Action Sheet

Tapping the reply arrow shows: Reply, Reply All, Forward, Trash, Remind Me, Flag, Mark as Unread, Move Message, Move to Junk.

Do not choose any action from this sheet unless the user explicitly asks. Treat Reply All and Forward as distinct actions; never infer either from a request to reply.

## Reply To A Message

1. Open only the relevant message and choose Reply only when requested.
2. Verify the intended thread and recipient without reporting unrelated visible content.
3. Focus the reply body, paste the reply using the text-entry steps, and call `screenshot(phone_id)` to verify the complete rendered reply.
4. Show the user the exact recipient, subject or thread, and final reply body, then request final confirmation to send. An earlier request to reply is not final send confirmation.
5. Do not tap Send until the user confirms after seeing that final preview.
6. After confirmation, re-check that the reply is unchanged, tap Send, and call `screenshot(phone_id)` to verify the result.

## Drafts

- Preserve a draft by default. Do not discard a draft merely because it was opened for inspection.
- If a compose sheet is empty, tapping **X** dismisses it without a discard confirmation. If text has been entered, expect a save/discard prompt; choose **Save Draft** unless the user has confirmed deletion of that exact draft.
- Before saving a prepared draft, verify its recipient, subject, and body and obtain authorization for the Save action.
- Before deleting or discarding any draft, show which draft will be affected and request confirmation immediately before deletion. An initial request to delete is not final confirmation.
- If deletion is not confirmed, leave the draft unchanged or save it only with authorization.

## Delete A Message

1. Identify the exact message using only the minimum relevant sender, masked address, subject, and date information.
2. Show the user which message will be deleted and request confirmation immediately before deletion. An initial deletion request is not final confirmation.
3. Do not tap Trash, Delete, or an equivalent control until that confirmation is received.
4. After confirmation, delete only the identified message and call `screenshot(phone_id)` to verify the result without reporting unrelated inbox content.

## Craigslist Workflows

### Craigslist Verification Email Workflow

Craigslist can send confirmation, account signup, posting, or verification email. These messages contain authentication links (such as a **complete account sign-up** button) and sometimes codes, so the absolute authentication boundary applies.

```
1. Open Mail -> screenshot to verify current screen
2. If in a message or compose sheet, back out to the inbox without discarding drafts
3. Check filter state if the expected email is missing
4. Search the narrowest relevant scope for craigslist (also try robot, confirm, posting, or verification)
5. Confirm a matching Craigslist sender/subject row exists without opening unrelated messages
6. Stop. Do not open the verification link, tap complete account sign-up, or read/enter any code
7. Hand the device to the user to open the message and complete the verification themselves
8. Resume only after the user confirms the flow is complete and a non-sensitive screen is visible; never screenshot while links or codes are visible
```

Observed characteristics of the Craigslist account signup email: it arrives immediately after the user requests a login link in Craigslist, comes from a Craigslist automated sender, indicates **craigslist account sign-up**, contains a prominent **complete account sign-up** button, and includes a **didn't request this link?** link. All of these controls are user-only; never tap them.

### Craigslist Reply Drafts

When Craigslist opens Mail from listing **reply -> email**, the compose sheet may already contain:

- A Craigslist relay-style recipient.
- The listing title as the subject.
- The Craigslist listing URL in the body.
- The normal Mail signature.

The body field is usually focused, and the send arrow may already be available. Do not tap Send without the final preview and fresh confirmation described above.

Preserve the draft by default. When leaving the composer, save the draft if prompted; never choose **Delete Draft** automatically. Before deleting, identify the draft without exposing the poster's address (for example, by the Craigslist listing title) and obtain fresh confirmation for that exact draft.

After sending from a Craigslist compose sheet, Mail may return to a previously open Mail message instead of returning to Craigslist. Do not summarize that message; reopen Craigslist directly if needed.

### Craigslist Listing Replies

Incoming replies can show in the inbox as `craigslist [listing id]` or from a Craigslist relay-domain sender. Reply subjects can use `Re: [listing title]`.

Search alert emails can look similar but are labeled **CL Search Alerts** and use subjects like `(# new results) [saved search name]`.

If a poster asks for screening details such as direct email address, pets, number of residents, or current housing status, summarize the ask for the user. Do not provide direct personal contact details or extra screening details unless the user approves the reply content.

## Guardrails, Tips, And Gotchas

- Treat Send and Delete as consequential actions that always require the just-in-time confirmations above.
- Dismiss or leave account-level prompts unchanged unless the user explicitly asks to change that setting and the action does not cross the authentication boundary.
- If the current screen, mailbox scope, recipient, or target message is ambiguous, stop and ask rather than acting on a guess.
- A bottom-left filter can hide messages; check for **Filtered by Unread** if a message seems missing.
- Search suggestions are not final results. Press the keyboard search key or choose a suggestion to run a search.
- Craigslist replies and search alerts can share the same Craigslist icon in the inbox; use sender and subject snippets to avoid opening the wrong item.
- Avoid opening unrelated personal messages. Search narrowly and verify sender/subject before opening.
