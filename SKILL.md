---
name: loadout-lab
description: Find the current meta build for any mobile game character, hero, or brawler. Use this skill when the user asks for the best build, loadout, items, gear, star power, gadget, equipment, ability, rune, or talent for a specific character in any mobile game — including but not limited to Brawl Stars, Mobile Legends, Honor of Kings, Clash Royale, MLBB, Wild Rift, AFK Arena, Free Fire, PUBG Mobile, Genshin Impact, and similar titles. Triggers on phrases like "best build for", "what gadget should I use", "best items for", "optimal loadout", or "meta build".
argument-hint: "[game name] [character name]"
author: Umang Srivastava
version: 1
---

# Loadout Lab

## Referenced Guides

| Reference | When to load | What it covers |
|---|---|---|
| references/search-sources.md | Before Step 2 (searching) | Which sources to search per game type, how to rank source reliability |
| references/output-template.md | Before Step 5 (responding) | Exact output format, section headers, tradeoff table structure |

Load each reference only when you reach the step that needs it.

---

## Step 1 — Parse and Validate the Request

Extract values from `$ARGUMENTS`:
- **Game name**: `$0` — the mobile game the user is asking about.
- **Subject**: `$1` — the specific hero, brawler, character, champion, weapon class, or deck card they want a build for.
- **Optional context**: Anything else in `$ARGUMENTS` beyond `$0` and `$1` — such as a game mode (e.g. "Heist", "Ranked"), a map name, a rank tier, or an enemy type. Store this as additional context and use it when building Option B (the situational variant) and when writing the How to Play section.

**Before proceeding, determine the game type:**

Games fall into these broad categories based on the name provided:
- **MOBA / hero-based** (Mobile Legends, Honor of Kings, Wild Rift, Vainglory): subject = hero/champion name
- **Brawler / hero shooter** (Brawl Stars): subject = brawler name
- **Battle royale** (PUBG Mobile, Free Fire, Call of Duty Mobile): subject = weapon type, loadout role, or character skill set — NOT a hero name
- **Deck / card game** (Clash Royale): subject = card or win condition, not a character
- **RPG / gacha** (Genshin Impact, AFK Arena, Raid: Shadow Legends): subject = character name

**Validation checks (adapt the question to game type):**

- If both game name and subject are clearly present, continue to Step 2.
- If only one value is given and it could belong to multiple games (e.g. "Layla" exists in Mobile Legends and Genshin Impact), ask: *"Which game are you asking about?"* Wait for their reply before continuing.
- If the game name is present but the subject is missing:
  - For MOBA / brawler / RPG games: ask *"Which character do you want the build for in [game name]?"*
  - For battle royale games: ask *"Which weapon class or loadout style are you asking about in [game name]? For example: sniper, close-range, or support."*
  - For card games: ask *"Which card or win condition do you want a deck built around in [game name]?"*
- If neither is clear, ask the user to provide both the game name and what they want optimized.
- If multiple characters or subjects are detected in `$ARGUMENTS` (e.g. "Colt and Shelly Brawl Stars"), handle them one at a time. Process the first character completely through Steps 2 to 5, deliver the result, then ask: *"Want me to look up the build for [second character] too?"*
- If the user has included a skill level or experience context in `$ARGUMENTS` (e.g. "for beginners", "for ranked", "for competitive"), store this. It will adjust the How to Play tips in Step 5.

**Do not proceed to Step 2 until both values are confirmed.**

---

## Step 2 — Load Search Sources

Load `references/search-sources.md` now.

Use the guidance in that file to identify the correct sources for the game type provided. Every search must cover at minimum:
1. A game-specific subreddit or community forum
2. A tier list or dedicated wiki site for that game
3. YouTube (recent guide videos)
4. Any official or community Discord mentioned in the sources guide

**Search queries to run (adapt `[GAME]` and `[CHARACTER]` to the actual values):**
- `[CHARACTER] best build [GAME] 2025` (or current year)
- `[CHARACTER] meta [GAME] reddit`
- `[CHARACTER] guide [GAME] site:youtube.com`
- `[CHARACTER] tier list [GAME]`
- `[CHARACTER] [GAME] build after update` (to catch recent patch changes)

Run at minimum **3 search queries** before moving to Step 3. For complex games with many build variables (e.g. items + runes + abilities), run up to 5 queries.

---

## Step 3 — Quality Gate (Mandatory — Do Not Skip)

Before accepting any build data, internally verify all of the following:

**Recency check:**
- Are the top results from the last 3 to 6 months? If all results are older than 6 months, flag this to the user and still provide the best available information with a confidence warning.
- Has there been a major patch or season reset recently that could have invalidated older guides? If yes, note this explicitly.

**Consensus check:**
- Do at least 2 independent sources (e.g. a Reddit thread AND a tier list site) agree on the core build for Option A? If not, run 1 to 2 additional searches to find consensus before proceeding.
- If sources actively conflict (e.g. one recommends gadget A and another recommends gadget B), note both perspectives under the relevant build option rather than silently picking one.

**Confidence self-assessment:**
Ask yourself: *"If I were a knowledgeable player of this game, would I be comfortable recommending this build without second-guessing?"*
- If YES — proceed to Step 4.
- If NO — run one more targeted search, then proceed regardless. Flag any remaining uncertainty explicitly in the response.

**Never present a build without completing this gate.** A slow but accurate answer is better than a fast but wrong one.

---

## Step 4 — Build the Three Options

Organize your findings into exactly three options. Do not present fewer.

**Option A — Meta / Most Reliable**
The build that the majority of experienced players and up-to-date sources recommend. Optimized for consistency and winning rate. When in doubt, default to what high-ranked players in the current season are running.

**Option B — Situational / Aggressive**
A variant that trades one of the Option A choices for a more aggressive or mode-specific selection. This could be a different gadget for a specific game mode, a different item for a specific enemy composition, or a talent/gear swap that amplifies burst damage.

**Option C — Off-meta / Niche**
A less commonly recommended build that some experienced players use for specific reasons — a surprise pick, a fun build, or one that counters specific matchups. Be clear about when and why someone would choose this over A or B.

For each option, list every build component the game uses (gadgets, star powers, gears, hypercharge, items, runes, talents, abilities, masteries, etc.) with a one-line reason next to each component.

---

## Step 5 — Format and Deliver the Response

Load `references/output-template.md` now.

Follow the template exactly. The response must contain:

1. **Character Overview** — One paragraph covering the character's role, playstyle, and what makes them unique in the current meta.

2. **Three Build Options** — Formatted as Option A, Option B, Option C per Step 4 above.

3. **Why This Works** — A plain-language explanation of how the components in Option A create synergy. Explain it like you're talking to a player who knows the game but hasn't thought deeply about this character.

4. **How to Play It** — 3 to 5 concrete in-game tips for executing Option A effectively. These should be actionable (what to do at the start of a match, when to use abilities, positioning, etc.), not vague advice like "play aggressively." **If the user specified a skill level** (e.g. "beginner", "new player"), keep tips simpler and avoid assumed game knowledge. **If the user specified "ranked" or "competitive"**, add a tip about team composition or draft context.

5. **Tradeoffs Table** — A markdown table comparing all three options across three dimensions relevant to the game. For most games: Ease of Use, Damage Output, Survivability. For support/healer characters, swap Damage for Healing/Utility. For battle royale games (PUBG Mobile, Free Fire), use Mobility, Firepower, Survivability.

6. **Source Confidence** — One line stating whether the build is confirmed by recent sources (last 3 months), older sources (3 to 6 months), or is partially uncertain with a note explaining why.

---

## Edge Cases

- **Game not recognized:** Tell the user clearly that this game isn't in your search sources and ask them to confirm the full game name. Offer to try a general web search anyway.
- **Character doesn't exist in that game:** Tell the user and ask them to double-check the character name or confirm which game it belongs to.
- **Character was recently released or reworked:** Flag this explicitly. State that build data may be limited or in flux and recommend the user cross-check on the official Discord or subreddit after receiving your answer.
- **No build data found after multiple searches:** Tell the user honestly. Do not fabricate a build. Instead, point them to the community resources from `references/search-sources.md` where they can find up-to-date information.
- **Major recent patch with no community guides yet:** Acknowledge the patch, provide the last known meta build, and strongly recommend verifying on the subreddit since the meta may have shifted.
- **User asks follow-up questions about the build:** Answer them directly using the same character and game context. Do not re-run the full search unless the user asks about a different character or game.
- **Game uses a different build vocabulary** (e.g. "Echoes" in Genshin Impact instead of "items"): Adapt your output format to use the correct game-specific terminology. Do not force generic terms onto games with their own systems.
