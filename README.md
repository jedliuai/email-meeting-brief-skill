> **Chatgpt代充：[aishopvip.com](https://aishopvip.com)**

# Email Meeting Brief Skill

A Codex skill for turning customer emails and meeting materials into printable bilingual meeting briefs.

This skill is designed for situations where a user needs to join a client call soon and must quickly understand what the customer asked, translate the material, prepare point-by-point answers, and print a practical Word document for use during the meeting.

## What It Produces

The skill guides an agent to create a `.docx` brief with:

- A meeting strategy box.
- Customer original text preserved in English.
- Chinese translation next to or below the original text.
- Point-by-point answer plans.
- Suggested commitments and dates.
- Risk reminders for each answer.
- Short English speaking lines for the call.
- An internal confirmation checklist.
- A printable layout verified by rendering the Word document to page images.

## When To Use

Use this skill when the user provides or points to:

- Exported email files such as `.eml`.
- Pasted customer email text.
- Meeting notes.
- Regulatory, technical, commercial, or project follow-up questions.
- Customer comments that require answers in an upcoming call.
- Source material that needs a Chinese translation and a practical response plan.

It is especially useful when the user says things like:

- "Read this email and translate it into Chinese."
- "Help me prepare answers for a customer video meeting."
- "Make a printable Word version."
- "Keep the English original so I can compare."
- "Tell me how to answer each point."

## Repository Contents

```text
email-meeting-brief-skill/
├── SKILL.md
└── agents/
    └── openai.yaml
```

## Installation

Copy this folder into your Codex skills directory:

```powershell
Copy-Item -Recurse . "$env:USERPROFILE\.codex\skills\email-meeting-brief"
```

Or, if your runtime uses another skill directory, place the folder there according to that runtime's skill-loading rules.

After installation, start a new Codex session so the skill metadata is loaded.

## Example Prompt

```text
Please read the email in this folder, translate it into Chinese, analyze how I should answer each customer point in tomorrow's video meeting, and generate a printable Word version. Keep the English original for comparison.
```

## Expected Workflow

The skill instructs the agent to:

1. Locate local source files before asking for connectors.
2. Parse `.eml` files with structured email APIs.
3. Extract the original text, sender, date, subject, and attachments.
4. Group the customer's questions by item number or topic.
5. Translate the original into Chinese while preserving important English terms.
6. Prepare a practical answer strategy for every issue.
7. Generate a Word document optimized for printing.
8. Render the Word document to page images and visually inspect the layout.
9. Revise the document if text overlaps, tables clip, or pages are too cramped.

## Privacy And Sanitization

This repository intentionally contains no customer email, meeting document, company name, product file, generated Word output, or project-specific example.

When using the skill, agents should avoid publishing or sharing source materials unless the user explicitly asks for that and confirms the materials are safe to share.

Before uploading any derivative package, scan for:

- Customer names.
- Company names.
- Personal names.
- Email addresses.
- Phone numbers.
- Project names.
- Batch numbers.
- File numbers.
- Local meeting documents.
- Generated customer-specific DOCX/PDF/PNG files.

## Design Notes

The skill is intentionally strict about layout verification. For printable Word documents, inspecting the generated `.docx` text is not enough. The agent must render the pages and check the images for overlap, clipping, unreadable fonts, cramped tables, and bad page breaks.

For dense meeting content, the skill recommends A4 landscape layout and compact issue cards instead of one oversized table that may break across pages.

## License

Use and adapt as needed for your own Codex skill workflows.
