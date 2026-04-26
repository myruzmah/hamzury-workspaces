# Hamzury Workspace System — Operational Spec

**For the Claude building the Hamzury website.** Read this end-to-end before touching any portal HTML. This document is the canonical description of what already exists, how it operates, and the design rules every workspace must obey. Deviation from these rules is a mistake.

Live demo: https://myruzmah.github.io/hamzury-workspaces/
Repo: https://github.com/myruzmah/hamzury-workspaces

---

## 1. The big picture

Hamzury is one company with six internal workspaces. Each workspace serves one role. The CSO (Customer Success Office) is the only team that talks to clients — every other team works behind the curtain.

```
                   ┌─────────────────────────────┐
   public sites ──▶│   CHAT WIDGET (one widget)  │── lead form ──▶ Sheet
   (4 of them)     └─────────────────────────────┘
                                 │
                                 ▼
                    ┌────────────────────────┐
                    │      CSO PORTAL        │  ← only client-facing team
                    │   (front office)       │
                    └────────────────────────┘
                                 │ assigns work to back offices
        ┌────────────────────────┼────────────────────────┐
        ▼                        ▼                        ▼
  ┌──────────┐            ┌──────────┐            ┌──────────┐
  │  BIZDOC  │            │  SCALAR  │            │ MEDIALY  │
  │ tax/comp │            │ web/auto │            │  social  │
  └──────────┘            └──────────┘            └──────────┘
                                                         │
                                                         │ briefs design/video work
                                                         ▼
                                              ┌──────────────────┐
                                              │      STUDIO      │ ← also gets work direct from CSO
                                              │   design+video   │
                                              └──────────────────┘

  ┌──────────────────────────────────────────────────────────────────┐
  │                   BUSINESS DEV (cross-cutting)                   │
  │   3 lanes — Marketing · Grants & Partners · Affiliates           │
  │   feeds leads + capital + reach into the rest of the system      │
  └──────────────────────────────────────────────────────────────────┘
```

The 4 public sites that the chat widget lives on:
- hamzury.com (parent / about)
- bizdoc.com (tax & compliance)
- scalar.com (web & automation)
- medialy.com (social media)

(HUB has its own site coming later — being built in a separate session.)

---

## 2. The six workspaces — at a glance

| # | Workspace | Login | Accent (portal) | Brand division color |
|---|-----------|-------|-----------------|----------------------|
| 1 | **CSO** | `CSO` / `CSO` | Warm gold `#B48C4C` | Gold (universal) |
| 2 | **Bizdoc** | `BIZDOC` / `BIZDOC` | Navy `#0F2C4A` (portal) | Forest Green `#1B4D3E` (public brand) |
| 3 | **Scalar** | `SCALAR` / `SCALAR` | Teal `#1F6B5C` | Golden Yellow `#D4A017` (public brand) |
| 4 | **Medialy** | `MEDIALY` / `MEDIALY` | Cream gold on dark `#E2C091` | Royal Blue `#1D4ED8` (public brand) |
| 5 | **Studio** *(new)* | `STUDIO` / `STUDIO` | Violet `#5B3FA8` | (internal — not a public division) |
| 6 | **Business Dev** *(new)* | `BIZDEV` / `BIZDEV` | Terracotta `#B8552E` | (internal — not a public division) |

**Important:** the **portal accent** and the **public brand color** are NOT always the same. Bizdoc's public site is forest green per the brand book, but the internal portal uses navy because it was the first portal built and that's what the founder approved. Same for Medialy (public = royal blue, portal = dark + cream gold). The portals are internal tools — they have their own elegant palette per workspace. **Do not "fix" the portal colors to match the public brand.**

The two newest portals (Studio, Business Dev) chose unused colors so each workspace stays visually distinct.

---

## 3. The shared design system — non-negotiable

Every portal must obey these rules. If any new file deviates, that's a mistake.

### 3.1 Typography
- **Sans:** SF Pro stack (Apple system font) — `-apple-system, BlinkMacSystemFont, "SF Pro Display", "SF Pro Text", "Helvetica Neue", Helvetica, Arial, sans-serif`
- **Serif:** Fraunces (loaded from Google Fonts) — used for hero titles, section headings, brand mark, panel titles, hero numbers. Italic variant for emphasis on accent words.
- **Mono:** JetBrains Mono — used sparingly for codes / IDs only.

**Rule:** sans for everything UI. Serif for emotional or hierarchy moments only. Mono for IDs only.

**Note about brand book vs portals:** the public brand book mandates "Inter only, no serif." That rule applies to **client-facing deliverables** (brochures, decks, proposals). The **internal portals** use SF Pro + Fraunces because the founder approved that combination through many design refinement rounds. This is intentional, not an oversight.

### 3.2 Spacing
- 8px grid throughout. All padding/margin/gap values are multiples of 4 (i.e. half-grid is allowed but never thirds).
- Hero padding: `28px 32px 60px` for the content area.
- Card padding: `18px 20px` for stat cards, `22px` for panel cards.
- Section gaps: `22px` between major blocks, `14px` between cards in a list.

### 3.3 Borders & lines
- **Hairline borders only.** `border: 0.5px solid var(--line)` everywhere. NEVER 1px or thicker.
- Border radius: 12px for cards, 10px for input/buttons, 8px for inner items, 999px for pills.

### 3.4 Color tokens (every portal must define these)
```
--bg          page background (lightest)
--paper       card / surface background (white)
--ink         primary text
--ink-soft    secondary text
--ink-muted   tertiary / labels
--ink-faint   placeholder / disabled
--line        hairline borders
--line-soft   even softer dividers
--sidebar     sidebar background (slightly tinted)
--accent      workspace primary
--accent-soft slightly lighter primary
--accent-pale very pale tint of primary (for hover, badges)
--success     green (#1A6B3C)
--warn        amber (#B8731F)
--danger      red (#A3301F)
--info        blue (#1A56C7)
--focus       rgba of accent at 0.14 alpha (focus ring)
```

### 3.5 The four-layer layout (every portal)
```
┌─────────────────┬──────────────────────────────────┐
│    SIDEBAR      │  HEADER (eyebrow + serif title)  │
│  (240px)        ├──────────────────────────────────┤
│                 │                                  │
│  brand          │           CONTENT                │
│  ─────          │      (scrollable)                │
│  section        │                                  │
│  nav-item       │                                  │
│  nav-item       │                                  │
│  ...            │                                  │
│  ─────          │                                  │
│  user / logout  │                                  │
└─────────────────┴──────────────────────────────────┘
```
- **Sidebar** — 240px wide, slightly tinted background, hairline right border, brand at top, sections of nav-items, user row + logout at bottom.
- **Header** — sticky top, hairline bottom border, blurred translucent background, eyebrow (uppercase 11px) above serif title (22px).
- **Content** — full scroll, 28px 32px 60px padding.
- **Below 900px** — sidebar collapses, hamburger button appears.

### 3.6 The hero pattern (every Home tab)
Every workspace home opens with:
1. A serif headline `38px`, line-height `1.05`, `Welcome to <em>{Workspace}</em>.`
2. One-paragraph sub `15.5px`, `--ink-soft`, max-width 580px.
3. Hairline divider.
4. (For multi-role workspaces only) Role toggle pill: `All | Role A | Role B | Role C`.
5. 4-card stats grid showing the most important live numbers.
6. A "Today's priorities" list (auto-generated tasks) with a "View all" pill button.

### 3.7 The 3-layer information system (the heart of every workspace)

Every workspace must implement these three layers:

**Layer 1 — My Tasks (auto-generated):** A list of tasks generated programmatically from the data state. Examples: "X is overdue", "New brief from CSO needs review", "Payout due in 3 days." These are NEVER manually created. Sorted by urgency: red (overdue/critical) → amber (this week) → green (later). Always a count badge in the sidebar.

**Layer 2 — Embedded checklists:** Inside every detail view (a brief, a project, a campaign, a grant), there's a sticky right-hand panel with a checklist appropriate to the current stage. Items can link to a Playbook guide via 📖 icon. Progress is calculated and shown as a thin bar. State persists in the item's `_cl` object.

**Layer 3 — Playbook tab:** A searchable knowledge library — every "how to" guide the team needs. Each guide has a tag, a name, a TL;DR (one line), and structured sections. Accordion-open one at a time. New hires productive day 1 by reading the Playbook.

**Linkage:** A checklist item can carry a `guide` ID. Clicking 📖 jumps to the Playbook tab. The same Playbook is the source of truth that checklists reference.

### 3.8 The login pattern (every workspace, identical)
- Centered login card on a soft radial-gradient background.
- Brand dot (small filled circle with single letter) + workspace name in serif.
- Eyebrow (uppercase, 11.5px, accent color) above the title.
- Serif title with the form `Sign in to <em>verb</em>.` — verb is workspace-specific (build / file / schedule / create / grow / ship).
- Username + password fields — both labels and both fields full width.
- Pill-shaped primary button.
- Below: hairline divider then a small hint sentence.
- Auth state in `sessionStorage` under a key like `studio_signed_in`.
- **Username and password are the same word**, both uppercased before comparison. So `studio` typed in the username field becomes `STUDIO` and matches `VALID_USER = 'STUDIO'`.

### 3.9 Voice rules
- **Calm, never urgent.** Never use `!` in UI copy except in error messages. Never ALL CAPS in body copy.
- **Short.** Section labels uppercase 11px with letter-spacing — short.
- **Confident.** "We deliver" not "We try our best to deliver."
- **No emojis in body copy.** Emojis ONLY in tag prefixes for clarity (e.g. 🎯 Marketing, 🌳 Grants, 🤝 Affiliate, 🎬 Video, 🎨 Design). No celebratory emojis. No 🚀.

---

## 4. The data flow between workspaces

This is the most important part. Each workspace is a node in a graph, with specific in/out edges. Get any of these wrong and the system stops working.

### 4.1 Inbound to CSO
- **From the chat widget on any of the 4 sites** → row in `Diagnostics` tab of Sheet → CSO Portal Inbox.
- **From a campaign landing page** (Business Dev) → row in `Diagnostics` with `source=campaign` → CSO Portal Inbox.

### 4.2 CSO → Back office (Bizdoc / Scalar / Medialy / Studio)
- CSO clicks "Assign to..." dropdown in the lead drawer.
- Backend writes a row to the appropriate `Briefs` tab (`bizdoc-briefs`, `scalar-briefs`, etc.).
- Back office workspace polls and shows the new brief in their Inbox.
- Back office accepts → ticks checklist → ships.
- Backend notifies CSO when shipped → CSO delivers final asset to client over WhatsApp/email.

### 4.3 Medialy → Studio (the special path)
This is the one most likely to be missed. Medialy plans monthly content for a client. When that content needs design (carousel, static post) or video (reel, B-roll edit), Medialy sends it to Studio:
- Medialy creates a content piece in their schedule.
- For each piece, Medialy ticks `design`, `recording`, `editing` checkboxes — these are tasks Studio fulfills.
- A brief lands in Studio's "Incoming briefs" with `source: 'medialy'`.
- Studio designer or video editor accepts, makes the file, ships back to Medialy.
- Medialy schedules and publishes the post.

### 4.4 CSO → Studio (the other path)
CSO can send Studio standalone work that doesn't go through Medialy. Examples: pitch deck, founder intro video, logo + brand sheet, one-off graphic.
- Brief lands in Studio's "Incoming briefs" with `source: 'cso'`.
- Studio fulfills, ships back to CSO.
- CSO delivers to client.

**Studio's incoming queue must visually distinguish the two sources** with colored "source strips." Medialy strip = royal blue. CSO strip = gold. This visual separation is a UX non-negotiable.

### 4.5 Business Dev → CSO
- Marketing campaigns generate leads → leads with status `new` are routed to CSO automatically.
- Once CSO contacts the lead, ownership flips to CSO and Business Dev only watches the funnel metrics.

### 4.6 Affiliates → CSO
- Affiliate's referral code attaches to the chat form / contact form submission.
- CSO sees the code on the lead in their Inbox.
- When CSO closes the deal and client pays, Business Dev's affiliate admin pays out commission (10% of first contract value, by 5th of next month).

---

## 5. Studio Portal — full operational spec

**File:** `studio.html` (in repo) / `studio-portal.html` (in package).
**Login:** `STUDIO` / `STUDIO`.
**Accent:** Violet `#5B3FA8` on milk-violet background `#FAF8FE`.
**Two roles share the workspace:** Designer + Video Editor. They both see the same Inbox; they self-assign by `discipline`.

### 5.1 The Studio sidebar
```
Workspace
  Home
  My tasks         (count badge — auto-generated)
  Incoming briefs  (count badge — anything queued)

Production
  Design queue    (kanban: queued → in-progress → review → shipped)
  Video queue     (same kanban for video)
  Ready to ship   (anything in review awaiting final QA)

Library
  Asset library   (every shipped deliverable, by client)
  Playbook        (knowledge guides)
```

### 5.2 The Studio data model
Every brief has these fields:
```js
{
  id: 'BR-001',
  source: 'medialy' | 'cso',     // who sent it
  sourceRef: 'MED-CT-201',       // their tracking ref
  discipline: 'design' | 'video',
  status: 'queued' | 'in-progress' | 'shipped',
  stage: 'designing' | 'editing' | 'review' | null,
  receivedAt, startedAt, shippedAt,
  clientName, clientNiche,
  type: 'Reel' | 'Carousel' | 'Pitch deck' | 'Logo' | ...,
  title,
  spec: { format, hook, cta, captions, music, ... },
  assets: ['file1.mov', 'file2.svg'],   // source materials sent in
  deadline: '2026-04-28',
  progress: 0..1
}
```

### 5.3 Studio's role toggle
Above the stats grid on Home and on My Tasks:
- `All | 🎨 Designer | 🎬 Video editor`
- Toggling filters tasks/briefs/queue counts to that lane only.
- The toggle is purely client-side — no separate logins per role. Both roles share the same workspace and credentials.

### 5.4 Studio checklists (4 of them)
1. **Design Brief Kickoff** — items shown when a designer accepts a queued brief.
2. **Design Internal Review** — items shown when a design moves to review stage.
3. **Video Brief Kickoff** — items shown when a video editor accepts a queued brief.
4. **Video Polish & Export** — items shown when a video moves to review stage.

The checklist that appears on a brief detail page is chosen by the brief's `discipline` AND `stage`.

### 5.5 Studio Playbook (8 guides)
- How to read a creative brief (reading discipline)
- How to use references without copying (taste)
- Music licensing rules (legal — only Epidemic / Artlist / cleared sources)
- Caption style — Hamzury house rules (typography, line length, brand color emphasis)
- How to handle revisions (2 rounds free, after that route through CSO/Medialy)
- File naming convention (lowercase, underscores, YYYYMMDD, vN, then "final")
- How to ship clean work (ship list per discipline)
- How to stay in flow (90-min blocks, no Slack, no lyrics)

### 5.6 Common mistakes in Studio implementations
- ❌ Using one queue for everything (designer and video editor get visually mixed work).
- ✅ Two queues — Design queue and Video queue — plus a unified Inbox where both can see everything.
- ❌ Treating Medialy briefs and CSO briefs identically.
- ✅ Show source with a colored strip; route differently in checklists ("Ship to Medialy" vs "Ship to CSO").
- ❌ Hiding the Asset Library because it's boring.
- ✅ Asset Library is critical — when starting new work for an existing client, the designer must be able to pull past brand files.

---

## 6. Business Dev Portal — full operational spec

**File:** `bizdev.html` (in repo) / `bizdev-portal.html` (in package).
**Login:** `BIZDEV` / `BIZDEV`.
**Accent:** Terracotta `#B8552E` on warm-cream background `#FAF6F2`.
**Three roles share the workspace:** Marketing + Grants/Partnerships + Affiliate Admin.

### 6.1 The Business Dev sidebar
```
Workspace
  Home
  My tasks         (auto-generated across all 3 lanes)

Marketing
  Campaigns        (live + planned + closed)
  Lead pipeline    (kanban: new → contacted → qualified → proposal → closed)

Grants & partnerships
  Grants pipeline  (researching → drafting → submitted → approved/rejected)
  Partners         (active + in discussion + cold leads)

Affiliates
  Affiliate roster (per-affiliate stats — referrals, qualified, closed, earned)
  Payouts          (pending + paid history, mark paid action)

Knowledge
  Playbook         (8 guides spanning all 3 lanes)
```

### 6.2 The Business Dev data model
Five entity types, each tagged with a `role` for filtering:
- **Campaigns** (`role: 'marketing'`): name, channel, target, spend/budget, leads/qualified/closed/revenue, status.
- **Leads** (`role: 'marketing'`): stage, source campaign, est value, current owner.
- **Grants** (`role: 'grants'`): funder, amount, deadline, fit, stage, status, notes.
- **Partners** (`role: 'grants'`): type, status, value description, contact.
- **Affiliates** (`role: 'affiliate'`): code, niche, referrals, qualified, closed, totalCommission, pendingCommission, status.
- **Payouts** (`role: 'affiliate'`): affiliate ID, amount, period, due date, status (Pending / Paid).

### 6.3 The Business Dev role toggle
Above stats on Home + My Tasks:
- `All | 🎯 Marketing | 🌳 Grants & Partners | 🤝 Affiliates`
- Toggling filters the auto-generated task list to that lane.
- Sidebar nav stays full so a user can manually jump to any tab regardless of toggle.

### 6.4 Business Dev checklists (6 of them)
1. **Campaign Launch** — pre-launch sequence (audience, budget, briefs, tracking, soft launch, monitor week 1).
2. **Campaign Close** — pull numbers, get CSO feedback, write retro.
3. **Grant Application** — eligibility, exec summary, full proposal, supporting docs, submit 48h early.
4. **Partnership Kickoff** — frame both sides' wins, warm intro, 1-paragraph pitch, MOU.
5. **Affiliate Onboard** — review fit, agreement, code, portal access, welcome pack, training call.
6. **Monthly Payout Cycle** — pull closed referrals, match codes, calculate, cross-check, pay by 5th.

### 6.5 Business Dev Playbook (8 guides)
- How to define a campaign audience (one sentence, one person)
- UTM tagging rules (lowercase, underscores, test in incognito)
- Campaign retro — 1-page template
- How to read a grant RFP (three reads, build a yes/no checklist)
- How to write a grant proposal (match RFP structure exactly)
- Partnership mutual benefit framing (both sides must win)
- Affiliate fit criteria (overlap + trust + recent activity)
- Commission calculation (10% first contract, paid by 5th of next month)

### 6.6 Common mistakes in Business Dev implementations
- ❌ Three separate logins for Marketing / Grants / Affiliate.
- ✅ One login, role toggle for filtering. Three small teams of 1-2 people each — one portal is plenty.
- ❌ Routing all leads to Marketing.
- ✅ Marketing only owns leads at `stage: new`. The instant a lead is contacted, ownership flips to CSO.
- ❌ Showing all 4 grant statuses with the same visual weight.
- ✅ "Rejected" grants stay in the list (institutional memory) but with desaturated color and no urgency badge.

---

## 7. The chat widget (already built, on the public sites)

**File:** `hamzury-command-center.html` and embed `hamzury-embed-chat.html`.
The widget is the same widget on all 4 public sites. The page where it lives determines the default brand and conversation entry point. It guides the visitor through one of:
- Clarity Session (14-question diagnostic)
- Service request (one of 24 services)
- Bizdoc 21-service catalog (when on bizdoc.com)
- Compliance Health Check
- Contact / general inquiry

Every submission writes a row to the `Diagnostics` Sheet tab → appears in CSO Portal Inbox within 60s.

**Mistake to avoid:** building a "Bizdoc-only" chat for the Bizdoc site. The chat is one widget, only the *default routing* changes per site. The visitor can still ask about Scalar from the Bizdoc site.

---

## 8. The auth pattern (DO NOT change)

```js
const AUTH_KEY = '<workspace>_signed_in';   // unique per workspace
const VALID_USER = 'STUDIO';                // workspace name uppercased
const VALID_PASS = 'STUDIO';                // same as user

// On submit, both inputs are .trim().toUpperCase() before comparison
```

This is **demo-grade auth** — no backend, no real security. It's a UX hint, not real protection. The real security is:
- The portals are not linked from any public page.
- The URLs aren't indexed (rel="noindex" on landing).
- Real auth comes later when the backend is wired up — see `INTEGRATION-SPEC.md` in the package.

**Do not** add OAuth, JWT, or any "real" auth to demo workspaces. Wait for the integration phase.

---

## 9. The hosting setup

- Deployed on **GitHub Pages** at https://myruzmah.github.io/hamzury-workspaces/
- Source repo: https://github.com/myruzmah/hamzury-workspaces (public)
- Files at root, so URLs are `/<workspace>.html` — short and clean.
- Static HTML only. No build step. No package.json. No bundler.

This means: any change to a portal HTML, push to `main` → live in <60s. No deploy command needed.

**Mistake to avoid:** adding a build pipeline (Vite, webpack, npm). The portals are intentionally single-file HTML. Each ~50-90KB. They load fast, run anywhere, and the user can save them locally if hosting goes down. Don't over-engineer.

---

## 10. What still needs to be built (track this)

- ✅ CSO Portal — done
- ✅ Bizdoc Portal — done
- ✅ Scalar Portal — done
- ✅ Medialy Portal — done
- ✅ Studio Portal — done (this session)
- ✅ Business Dev Portal — done (this session)
- ⏳ **Affiliate Portal** — NOT YET BUILT. External-facing, per-affiliate logins (each affiliate has unique code as username). Affiliate sees: their referrals, commission earned, payout history, marketing assets to share. Distinct from Business Dev's affiliate-admin view.
- ⏳ **HUB Portal** — NOT YET BUILT. School workspace. Students + instructors. Cohort enrollment, course progress, certifications. Being built in a separate session because of scale.

When Affiliate is built, login pattern is per-affiliate: `DANIELO`/`DANIELO`, `FUNMIA`/`FUNMIA`, etc. — each affiliate's `code` doubles as both username and password (demo auth).

---

## 11. Frequent mistakes the website Claude is likely making

These are educated guesses based on common patterns. The Claude reading this should self-audit against each:

### 11.1 Visual mistakes
- **Using Inter for portals.** Wrong. Inter is for client deliverables. Portals use SF Pro + Fraunces.
- **1px borders.** Wrong. All borders are 0.5px hairlines.
- **Bright/saturated accents.** Wrong. Each portal uses a refined, slightly desaturated accent.
- **Adding emojis everywhere.** Wrong. Emojis only in tag prefixes (🎯 🌳 🤝 🎬 🎨), never in body copy.
- **Drop shadows on cards.** Wrong, mostly. Cards rely on hairline borders. The only shadow is the very subtle hover shadow `0 2px 6px rgba(0,0,0,0.04)` and only on hover.

### 11.2 Structural mistakes
- **Putting the brand at the top of the content area.** Wrong. Brand goes in the sidebar top, not in the content area.
- **One big nav bar instead of sidebar.** Wrong. Sidebar is the rule.
- **Mixing tabs with sidebar nav.** Allowed only inside detail views (a brief, a project, a campaign). The main nav is sidebar-only.

### 11.3 Behavioral mistakes
- **Not auto-generating My Tasks.** Wrong. My Tasks is computed from data state (incoming, overdue, due-soon, payouts pending, etc.). Never manually entered.
- **Hardcoding sample data into the UI.** Wrong (well — for now it IS hardcoded for the demo, but it's structured as a top-level constant array per entity type, ready to be swapped for `fetch()` calls when the backend wires up — see `INTEGRATION-SPEC.md`).
- **Forgetting the 60-second poll.** When backend is wired, every workspace polls fresh data every 60s. Don't build something that requires page refresh.

### 11.4 Hosting mistakes
- **Adding a build step or framework.** Wrong. Single-file HTML. No npm. No Vite.
- **Splitting CSS or JS into separate files.** Wrong (for now). Each portal is one self-contained file. This is intentional — it makes the portals portable, debuggable, and impossible to break by a missing dependency.

### 11.5 Brand mistakes
- **"Fixing" the portal accent to match the public brand color.** Wrong. The portal accents are intentionally chosen to be distinct internal colors. Bizdoc's portal is navy not green. Medialy's portal is dark + cream not bright royal blue.
- **Adding logos from the brand book to the portal sidebars.** Wrong. The portals use a simple "brand-dot" — small filled circle with a single serif letter. That's the entire portal logo language.

---

## 12. The voice of the system, in one paragraph

Hamzury portals feel calm. They use whitespace as luxury. They say less. They treat the user as a competent adult who doesn't need encouragement, animation, or celebration. They put the right information at the right level: numbers up top, tasks below, detail behind a click. Every screen has one focus. Every action has one button. There are no progress bars unless work is genuinely partial. There are no notifications unless something needs attention. The system breathes. The user does the rest.

If a screen feels busy, simplify it. If a label needs more than 3 words, it's the wrong label. If a button has more than 2 words after the verb, it's the wrong button. If the page has more than 5 colors visible at once, something is wrong.

---

## 13. Questions to ask the founder before deviating

If the website Claude is tempted to change any of the above, ask these questions first (and don't proceed without an answer):

1. "Should the portals match the public brand colors exactly, or stay with their refined internal palette?" *(Answer is currently: stay refined.)*
2. "Should I switch to Inter for consistency with the brand book?" *(Answer: no — portals use SF Pro + Fraunces by founder approval.)*
3. "Should I add a build pipeline?" *(Answer: no — single-file HTML for now.)*
4. "Should the chat widget be different per division?" *(Answer: no — one widget, default routing changes by site.)*
5. "Should I add real auth?" *(Answer: not yet — wait for the integration phase per INTEGRATION-SPEC.md.)*

---

## 14. The integration phase (next, not now)

The portals currently use hardcoded sample data. The next phase wires them to a Google Apps Script backend that reads/writes a Google Sheet. The full plan is in `INTEGRATION-SPEC.md` and `LAUNCH-PROMPT.md` in the original package. Don't start that phase from this document — read those documents instead.

The order is strict:
1. Backend deployed first (Apps Script as web app).
2. Chat widget wired (visitor → Sheet flow proven).
3. CSO Portal wired (read leads, write assignments).
4. Bizdoc / Scalar / Medialy wired one at a time.
5. Studio wired (read briefs from both Medialy and CSO).
6. Business Dev wired (campaigns, grants, affiliates writing back).

Each step requires the previous step to be confirmed working.

---

## 15. One-line summary of every mistake-class

> When in doubt, **the existing portals are correct**. Don't "fix" them. Match them.

Built to last.
