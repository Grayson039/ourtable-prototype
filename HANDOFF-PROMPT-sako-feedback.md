# Handoff Prompt — Sako Feedback + Status Audit (Aftershow & OurTable)

> Paste everything below the line into the earlier session that has the real context.

---

I'm Will. Sako (iOS dev on both projects) sent feedback notes on **Aftershow** and **OurTable**. A different Claude session read the repos and triaged the notes, but that session is NOT up to date on either build. You are. I need you to do two things, in this order.

## Rules (apply to everything below)
1. Check my Obsidian vault first for project context (Master Index → project folder → `_Index.md` status doc and `Notes.md` log). Don't ask me to re-explain what's already logged.
2. Only append to existing vault files, and only when a real decision, fix, scope change, or next step happened. Don't create new files.
3. Before saying anything is built or pushed, verify it's actually live. Code written is not the same as shipped.
4. Run a quick security check before calling anything done: could this expose credentials or data, or let someone other than me act on my behalf?
5. Nothing publishes, sends, pushes, or ships without my explicit approval.
6. Aftershow phase gates are hard stops (see its CLAUDE.md). Announce transitions, update the README log, and confirm with me first.

## Task 1 — Audit and report the ACTUAL current status of both builds
Go through the vault logs, both repos' git history, HANDOFF.md, README.md, and CLAUDE.md. Report what is true today, flagging anything stale or contradictory. Don't trust the docs over the code or the logs.

**Aftershow** (`Grayson039/aftershow-dev`)
- Current phase and real status. CLAUDE.md says Phase 3 in progress. HANDOFF.md (May 18, 2026) says all 14 screens and the prototype shell are done. Which is current?
- Which framework is the Phase 4 target? HANDOFF.md says SwiftUI (Sako is an iOS dev). CLAUDE.md and README.md still say React Native. Reconcile them.
- Is there a newer build than the static `aftershow-phase3-*.html` mockups plus `aftershow-prototype.html`? Sako's notes mention a rotating dial/knob vote, a ticket stub, and poster text scaling, and I couldn't find those in the onboarding or log-a-show mockups. Say where they live, or whether those features exist yet.
- Is there a Figma file, deployed prototype URL, or TestFlight for Aftershow? What's live?

**OurTable** (`Grayson039/ourtable-prototype`, working app in `screens-new/`)
- Where Phase 6 (EAS Build & device testing) really stands. `screens-new/eas.json` exists, so did a build run?
- Is the Supabase schema (`supabase/migrations/001_schema.sql`) actually applied to the real project? Docs say "pending / needs verifying".
- Which screens use real Supabase data and which still use mock data (`constants/data.ts`)?
- What is the "dining animation at top"? Sako's note says to build everything around it, and I couldn't find it in the code.
- Does "Recipe Rescue" exist anywhere (screen, feature, Figma)? I found nothing in the app code.
- The fridge "Scan" button currently routes to `/add-recipe`. Confirm whether any real scan/vision/AI code exists anywhere.
- Is there a web app at `C:\Users\Will\OurTable` separate from this repo? Which is the source of truth?

For each project, give me: current phase, what's verified live, what's code-only, what's stale in the docs, and what's blocked.

## Task 2 — Update the docs and the vault with that real status
After reporting, and after I approve, update:
- the repo `HANDOFF.md`, `README.md` log, and `CLAUDE.md` phase tables where they're wrong
- the vault `_Index.md` status doc and `Notes.md` log (append only)

Show me the diffs first. Do not commit or push until I say so.

## Sako's raw notes (verbatim)

### Aftershow notes from Sako
- [ ] Bottom of onboarding have copy that says anytime change theme
- [ ] privat side you can see and vote butnothers
- [ ] cant change copy
- [ ] The borders on any highlighted box corners are cut off a bit.
- [ ] Onboarding: add a screen that says "are you ready to log your first show? No, you go to profile, yes you go to show logging.
- [ ] Move the copy in the dial vote to underneath the dial
- [ ] City gets prefilled by google and states should be a drop down
- [ ] Text scale factor for logging a show. : you can be at maximum this many characters and shrink the font to fit the poster
- [ ] I wnt the text centered between venue,city and add a photo
- [ ] For the knob, have the knob in a frame so its not moving.
- [ ] Really cool experience: make the bottom of ticket stub a black to clear opacity gradient
- [ ] Filter newest first should also be a pill
- [ ] Stats visualization change it to a stacked bar graph
- [ ] Solve how to get artist photos and info about the artist. Maybe link to setlistfm, link to socials,wikipedia
- [ ] Maybe feed item: start following people to get their feeds
- [ ] Onboarding should have an add your location with description for shows nearby
- [ ] For voting should have a particle effect amplifier lightning bolts
- [ ] Move top photos to be the very first thing in the feed
- [ ] Click anywhere on the card go to show click on ofp go to profile
- [ ] Constraints on safari for movile make the menu
- [ ] Move log show to be part of the header or top right

### OurTable notes from Sako
- [ ] on grocery list add a quantity and title of item
- [ ] Search ingredients add a drop down so they can choose make it a list view so they
- [ ] Make everything revolve around the dining animation at top
- [ ] Actually work on the ai for scan your fridge, add confirm of stuff added
- [ ] No need for rescipe rescue

## First-pass triage (from a session that had NOT seen your logs — verify against what you know)

**Aftershow**
- Quick polish: clipped corners on highlighted boxes (likely `overflow:hidden` vs `border-radius`); dial-vote copy under the dial; "newest first" as a pill; "change theme anytime" copy at the bottom of onboarding (step 1 already says "You can change it anytime in Settings"); center text between venue and city; ticket-stub black-to-clear gradient; top photos first in feed; whole card tappable (to show) with the profile tap separate.
- Real design work: new onboarding screen "ready to log your first show?" (No → Profile, Yes → Log); onboarding location step ("shows near you"); stacked-bar stats chart; fixed-frame knob; feed empty state "follow people to see their feed"; city and state dropdown with Google prefill; poster text max-chars + shrink-to-fit; Safari mobile constraints (`100dvh`, safe-area insets).
- Push back on: moving "Log show" to the header, since it conflicts with the design principle "all flows lead to the + button" and the center Tab 3 CTA, so test it before committing. Lightning-bolt particles are a P2 delight feature, so defer.
- Artist photos and info: Setlist.fm has no images and restrictive API terms. Options are MusicBrainz/Wikipedia/Wikidata for links and some images, or the Spotify API for artist images. The cheap MVP answer is links out (Setlist.fm, socials, Wikipedia) and images later.
- Unclear, ask Sako: "privat side you can see and vote butnothers" and "cant change copy".

**OurTable**
- Grocery list: separate quantity and title fields. The data model already has `name` and `qty`, so this is mostly UI.
- Ingredient search: dropdown / list-view picker.
- "Revolve around the dining animation": unknown, I need a screenshot or Figma link. It's the biggest scope item.
- AI fridge scan with a confirm step: the largest build. **Security:** any Vision or Claude key must live in a Supabase Edge Function, never in the app bundle.
- Remove Recipe Rescue: easy, once we know where it is.

## What I want back
1. The status audit (Task 1) first, as a short table per project.
2. Your corrections to the triage above, since you know things the other session didn't.
3. A recommended order of work that respects the Aftershow phase gates.
4. The proposed doc and vault updates (Task 2) as diffs, awaiting my approval.
