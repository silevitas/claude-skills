---
name: "company-values-check"
description: "Research a company against the user's own values (business, money, leadership, customers, political activity) and return a sourced scorecard. Use for /company-values-check [company] or 'vet this company'."
---

# Company Values Check

Produce a sourced scorecard showing how a company lines up with the user's *stated* values, so they can decide whether to work with or for it. You report evidence and map it to their values. You do not decide for them, and you never rate against your own views or a generic "ethics" standard.

## Input

- Company name from the args.
- If the name is ambiguous (several companies share it, or it is a subsidiary or brand), ask one question to pin it down (website or HQ) before researching.
- Optional format override: `md`, `markdown` or `html` in the args wins for this run only. It does not change the saved default.

## Step 1: Load the values profile

- If memory tools are available, read `/topics/work-values.md` and `/topics/company-values-check-settings.md` in one call.
- If the values profile exists, list the red lines you will check against in 2-3 lines and continue without asking anything.
- If it does not exist, or the user says "update my values", run the interview below.
- Output format, in priority order: args override, then the saved `format` setting, then ask question 6 below once (alone, if the profile already exists) and save the answer. If memory tools are unavailable or the user does not answer, use Markdown and mention the `html` arg in the final reply.
- "Change my report format" updates the saved setting without rerunning the interview.

### Interview (max 6 questions; AskUserQuestion takes 4 per call, so ask 1-4 in the first call and 5-6 in a second)

1. **Hard no's** (multi-select): weapons or defense contracting; law enforcement, immigration enforcement or surveillance tech; fossil fuel extraction; private prisons or detention; gambling, tobacco or predatory lending; data brokering or ad surveillance; Other (free text).
2. **Political activity that concerns you**: company PAC giving, lobbying for specific legislation, trade-association membership. Ask which issues matter and *which direction* concerns them (e.g. reproductive rights, LGBTQ+ rights, labor and unions, climate, immigration, Israel and the Middle East, voting rights). Never infer a stance from a party label.
3. **Positive signals** that count in a company's favor (e.g. unionized, B Corp, nonprofit customers, credible climate commitments).
4. **How far association counts**: (a) the company only; (b) plus executives' and board members' professional roles and affiliations (default); (c) plus their personal political donations.
5. **Source rules**: any sources to exclude or distrust, in general or for specific topics.
6. **Report format**: Markdown file (Recommended) or HTML file.

Afterwards, summarize the profile in 5-8 bullets and ask the user to confirm. If memory tools are available, save in two separate writes, settings first:

1. `/topics/company-values-check-settings.md`: the `format` line only. This is not sensitive, and keeping it in its own file means it survives even if the values save is held.
2. `/topics/work-values.md`: the values profile. Political views are a sensitive category, so the save may be held by the user's memory settings; if it does not persist, say so in one sentence and continue with the profile for this session.

## Step 2: Research

Run searches in parallel. Prefer primary sources. Cover each dimension:

1. **Business**: what it sells, to whom, how it makes money. Company site, 10-K Item 1 on SEC EDGAR (public companies), press releases.
2. **Money**: (a) what it invests in or acquires: 10-K, corporate venture arm portfolio, Crunchbase, press. (b) who funds or owns it: major VCs, PE owners, large shareholders.
3. **Leadership**: CEO, exec team, board. Other board seats (DEF 14A proxy for public companies, org websites, LinkedIn), think tank or advocacy roles, published political writing, and personal FEC contributions only if the profile's association scope is (c). Sources: FEC.gov, OpenSecrets.
4. **Customers**: named customers and case studies, industries served, government contracts (USAspending.gov, SAM.gov), especially with agencies or industries on the user's hard-no list.
5. **Political activity**: corporate PAC (FEC, OpenSecrets), lobbying disclosures (Senate LDA database, OpenSecrets), trade associations, public positions on legislation or ballot measures.

Add labor record (NLRB cases, union status) or other areas only if the profile mentions them.

### Research rules

- Every claim needs a source URL. No source, no claim.
- Tag each finding: [Certain] for primary documents or official records, [Likely] for reputable secondary reporting. If you would be guessing, mark the item Unknown instead of writing it.
- Date every finding. Flag anything older than about three years as possibly stale.
- When both exist, give the company's own framing alongside criticism.
- Honor the profile's source exclusions.
- Before attributing a donation or affiliation to a person, confirm it is the same individual (employer, city). Common names produce false matches.
- Scope is people in leadership roles and their public professional or political activity. Never include family members, home addresses, or personal life details.
- Private companies usually have thin public data. Say so; do not pad.

## Step 3: Rate each dimension against the profile

- **Conflict**: evidence hits a stated hard no or red line.
- **Mixed**: some concern, partial overlap, or weak or indirect evidence.
- **Aligned**: matches stated positive signals and hits no red lines.
- **Unknown**: not enough public information.

No numeric score, no overall verdict, no recommendation.

## Step 4: Output (a file, never the full report inline in chat)

File name: `[company-slug]-values-check-[YYYY-MM-DD].md` or `.html`. If a folder on the user's computer is connected, save it there; otherwise deliver it as a file in the chat.

### Markdown file (default)

Use this structure:

```
# [Company]: values check
[one line on what they do] · Public/Private · HQ · Checked [date]

## Red-line hits
- Each Conflict finding in one line, or "None found" (note that none found is not proof none exist)

## Scorecard
| Dimension | Rating | Key finding | Confidence |
|---|---|---|---|
| Business | | | |
| Money (investments & funding) | | | |
| Leadership | | | |
| Customers | | | |
| Political activity | | | |

## Key findings
### Business
- Finding, dated [Certain] [1]
(repeat per dimension)

## Gaps
- What could not be verified

## Questions to ask them
- 2-4 questions the user could put to a recruiter or hiring manager to close the gaps

## Sources
1. [Title](url), publisher, date
```

### HTML file

Same sections and order as the Markdown structure, as one self-contained HTML file:

- All CSS inline; no external scripts. Google Fonts is the only allowed external resource, and a system font stack is fine.
- Header block with company name, one-line description, public/private, HQ, date checked.
- Red-line hits in a highlighted callout at the top.
- Scorecard as a table with rating chips. Each chip shows the text label (Conflict, Mixed, Aligned, Unknown) as well as a color, so meaning never depends on color alone.
- Footnote markers in findings link to the matching entry in Sources; source titles are clickable and open in a new tab.
- Light and dark themes via `prefers-color-scheme`, a readable layout at phone width, and print styles that keep the table and sources legible.
- If the Artifact tool is available, offer in one line to publish it as a private shareable page. Do not publish without a yes.

### Chat reply

Keep the chat reply to 1-3 lines: the number of red-line hits (or "none found"), the dimensions rated Unknown, and the file. Do not paste the report into the chat.

## Style

- Neutral and factual. No loaded adjectives ("troubling", "admirable"). Let the mapping to the user's values do the work.
- Use em-dashes sparingly.