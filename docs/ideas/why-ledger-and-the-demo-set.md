# Why LEDGER, and how we show it (Elmir, Nov 21)

## The idea in one breath
When a tenant moves out, the landlord decides what to keep from the deposit, often from memory. LEDGER films the apartment at move-in and at move-out, pins every frame of both videos to the same floor plan, and writes a draft itemized deposit statement. **It won't charge what it can't prove:** a charge needs at least two before/after frame pairs that agree it is the same spot and that it changed. Otherwise the surface is *not provable* and costs $0. A moved chair costs $0. In New York the landlord has 14 days to send that statement (NY GOL 7-108).

Problem Statement One ("5 Sigma"): a task today's agents get wrong (comparing two walkthroughs shot from different angles and in different light), done reliably, measured against a one-call Neon baseline that also gets the plan.

## Why this idea and not the others
We stress-tested seven finalists against the official judging (live demo 35%, proof 25%, creativity 20%, Gemini 4 + Decisions API 20%):

| Idea | Why not |
|---|---|
| Tripwire (Decisions API safety gate on a browsing agent's clicks) | Google already ships it: Computer Use has a per-action allow/confirm/block decision and a prompt-injection screenshot check. DeepMind judges would see their own feature. |
| Detonator (open a scam QR/link in a sandbox, verdict from screenshots) | Phishing kits hide from sandboxes, so a judge's live link often shows a decoy. Managed Agents have no built-in browser. urlscan.io and others cover most of it. |
| Leak Hunter (redact PII, then attack the output) | Safest to build, strongest proof, but little wow (black boxes on a page) and close to the banned "image analyzer". |
| Hearing Ledger (promises in city hearing video) | citymeetings.nyc already does timestamped NYC Council hearings; no ready labels for "commitments". |
| Policy Compiler (live multi-stream audio moderation) | Safety filters may refuse the toxic audio it exists to flag; phones in one room hear each other; many existing products. |
| Intent Interlock (voice commands checked slot by slot) | Its benchmark has been public since 2020, and it looks like Alexa. |

LEDGER had the highest ceiling: the best live moment (the judge changes the set with their own hands, then sees red pins land on that spot in 3D), a clear proof story, every model with a real job, and the Decisions API at its core. Its main risk was cross-angle change detection, which we addressed with the plan-anchored design, the two-pair proof rule, and a measured test set.

## Why a cardboard model apartment, not a real room
LEDGER needs a floor plan that matches the filmed space, a real change made by the judge between two videos, and a space the AI can read. A tabletop model gives all three in about a minute:

| Real room (e.g. the judging room) | Cardboard model of Unit 4B |
|---|---|
| Room unknown until judging starts | Built in advance |
| No floor plan; would have to draw and parse one live | Plan already in the code (`demo-table`), wall IDs match |
| Cannot damage the venue | Dry-erase scratch and sticker stains, reset in seconds |
| Filming a room twice plus processing does not fit 3 minutes | Two 10-second pans |
| Window light, passers-by, identical plain walls | One lamp, one landmark per wall |
| A pre-recorded room breaks the "no scripted inputs" rule | The judge's change is live and unrehearsed |

We still film 1-2 real-room pairs at the venue (sticker "damage") and report them as a separate row, so the answer to "would this work in a real apartment?" is a measured number, not a promise.

## The set (Unit 4B at 1 cm = 10 cm)
- Living room 30 x 25 cm, bedroom 25 x 25 cm, walls 25 cm high, one shared wall.
- Living room: entry door in the south wall, window in the north wall, door to the bedroom in the shared east wall. Bedroom: window in the east wall.
- One landmark on every wall without a door or window (mini poster, outlet sticker, coloured dot): our Decisions spike showed similar plain walls get confused (`docs/ideas/decisions-before-after-spike.md`).
- Plain matte walls, one lamp, phone exposure locked, a tiny chair. "Damage" = ~1 cm dry-erase line or brown sticker.

## Why the printed kit (`eval/kit/demo-table-kit.pdf`)
- **Page 1, the plan:** the blueprint for building and labelling the set, and on stage the judge sees which wall is which.
- **Page 2, judge cards:** the judge writes what they changed **before** any result appears; at the end the card sits next to LEDGER's verdicts. That is the proof it isn't staged. The same cards are the ground truth for the test pairs that people from other teams stage (the rules ban an eval made only of our own examples). Every wall is pre-printed with its surface ID, so what people write maps 1:1 to the scorer.
- Print 1 plan page and about 10 card pages (20 cards: ~18 test pairs + live judges).
