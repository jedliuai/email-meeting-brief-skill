---
name: email-meeting-brief
description: Use when a user needs to read customer emails or meeting materials, translate them, prepare point-by-point meeting answers, and create a printable Word/DOCX brief for a client call.
---

# Email Meeting Brief

## Purpose

Turn customer emails, exported `.eml` files, PDFs, DOCX files, or pasted meeting notes into a practical meeting brief the user can print and use during a live customer call.

The result must help the user answer each customer point, not merely summarize the email.

## Required Output

Create a printable `.docx` meeting brief unless the user explicitly asks for another format. The document must include:

1. **Meeting strategy box**
   - Overall stance.
   - Core negotiation/regulatory/technical message.
   - Suggested commitment-date logic.
   - Any warnings about promises that are overdue or risky.

2. **Original text + Chinese translation**
   - Preserve the customer's English original text.
   - Put the Chinese meaning directly beside or immediately below the original.
   - Use a structure that survives printing. If a wide comparison table becomes cramped, use one card/table per issue instead of one huge table.

3. **Point-by-point answer plan**
   For each customer item, include:
   - Item number or topic.
   - Customer concern.
   - Recommended Chinese answer.
   - Commitment/action/date to offer.
   - Risk reminder.
   - Short English sentence the user can say in the meeting.

4. **Internal confirmation checklist**
   - Questions the user should confirm before the meeting.
   - Items that affect whether the user can safely commit.

5. **Opening English lines**
   - 2-4 concise sentences the user can say at the beginning of the call.

## Workflow

### 1. Locate and Parse Source Material

- Search the working folder first.
- For `.eml`, parse with Python `email` APIs rather than manually decoding MIME.
- Extract:
  - Subject, sender, date, recipient.
  - Plain text body, falling back to HTML-to-text only if needed.
  - Attachment names and relevance.
- If the email is only in Gmail/Outlook and not local, use the appropriate connector only after it is available/authorized.

### 2. Identify Meeting Items

Group by the customer's numbering and headings. Preserve original labels such as `Item 1`, `13.a`, `Q-02`, or the customer's own section headings.

For each item, determine:

- What the customer is actually asking.
- Whether the customer asks for a comment, document, revised specification, template update, date, or explanation.
- What was already promised in previous meeting notes.
- Whether a promised date has already passed.
- What risk exists if the user gives a weak or overconfident answer.

### 3. Create Answer Strategy

For every item, write a usable answer, not a generic summary.

Use this pattern:

```text
Customer concern: What they are asking and why it matters.
Recommended answer: What the user should say.
Commitment/date: What to provide and by when.
Risk reminder: What not to say, or what must be confirmed internally.
English speaking line: A concise phrase suitable for the call.
```

When dates are missing:

- Propose conservative placeholder dates based on current date and workload.
- Mark them as suggested placeholders.
- Tell the user to replace them with company-approved dates before the call.

When prior commitments are overdue:

- Do not hide the missed date.
- Recommend acknowledging it briefly and giving a concrete recovery date.
- Avoid saying a document is complete unless the source material proves it.

### 4. Translation Rules

- Translate for meeting usefulness, not word-for-word legalistic prose.
- Keep product names, regulation names, file numbers, pharmacopoeia references, batch numbers, and test names in English where useful.
- Translate the meaning in Chinese, but preserve quoted test names exactly, e.g. `"Constituted Solution"`, `"Clear and Color"`.
- If dates in the email conflict with the current email date, explicitly note both absolute dates.

### 5. DOCX Layout Rules

Use a professional, print-oriented layout:

- A4 landscape is preferred for dense meeting briefs.
- Margins around 1.0-1.2 cm.
- Body font around 8.5-9.5 pt; do not go smaller unless unavoidable.
- Use clear section headings and shaded label cells.
- Avoid giant tables that cross pages badly.
- Prefer compact issue cards:
  - Blue header row for item title.
  - Two-column rows for customer concern and recommended answer.
  - Rows for commitment/date and risk reminder.
  - A full-width row for English speaking line.
  - Optional blank row for meeting notes.
- Keep enough white space for handwritten notes.
- Footer should include print suggestion, e.g. `A4 landscape, duplex print`.

## Verification Gate

Do not claim the Word document is ready until it has been rendered and visually checked.

For DOCX output:

1. Render the DOCX to page images using the Documents skill renderer, preferably `render_docx.py --renderer artifact-tool`.
2. Open and inspect the PNG pages.
3. Check for:
   - Text overlap.
   - Table clipping.
   - Content cut off at page boundaries.
   - Font too small to read.
   - Excessive blank pages.
   - Overly cramped tables.
4. If layout fails, revise the DOCX and render again.

If rendering reports a non-zero exit but still creates page images, inspect the images. If they are clean, mention only if relevant; do not block delivery solely because the renderer returned a noisy status.

## Common Mistakes

- Summarizing the email but not telling the user how to answer each point.
- Removing the English original, which makes live cross-checking difficult.
- Giving confident regulatory/technical commitments without flagging internal approval.
- Ignoring overdue promised dates.
- Making one huge table that looks acceptable in XML but clips when printed.
- Delivering DOCX without rendering and visually checking page images.

## Minimal User-Facing Final Response

Keep the final response short:

- Link the final `.docx`.
- State that English originals, Chinese translations, and point-by-point answer strategy are included.
- State whether rendering/visual check was completed.
- Flag any placeholders the user must confirm before the meeting.
