# ⚔️ Loadout Lab

**Name a character and a game. Get the current meta build, two alternatives, and a plain-language reason why it works.**

![Agent Skill](https://img.shields.io/badge/Agent_Skill-SKILL.md-6C47FF?style=flat-square)
![Built for](https://img.shields.io/badge/Built_for-BlueAI-0A84FF?style=flat-square)
![License](https://img.shields.io/badge/License-MIT-blue?style=flat-square)

Loadout Lab is an agent skill (a `SKILL.md` plus two reference files). Ask for the best build for any character, hero, brawler or weapon class in a mobile game. It searches community sources, runs a quality check on how recent and how consistent they are, and replies in one fixed format with three build options and an honest confidence rating. It was first named Pro Guide Finder.

<!-- TODO: add a screenshot or GIF of the skill in action here -->
<!-- TODO: add a link to the demo video here -->

---

## Try it

> Best build for Chrono in Free Fire
>
> What gadget should I use for Colt in Brawl Stars?
>
> Best build for Jinx in Wild Rift for ranked

You can add context like a game mode, a rank, or "for beginners", and the tips adjust. It also triggers on phrases like "best items for", "optimal loadout" and "meta build". If the game or the character is missing, or a name could belong to two games, it asks one question first.

## What you get back

| # | Section | Details |
|---|---|---|
| 1 | **Character Overview** | Role, playstyle, and where they sit in the current meta |
| 2 | **Three Build Options** | **A**: meta pick, the most reliable. **B**: situational or aggressive. **C**: off-meta or niche. Every slot has a one-line reason |
| 3 | **Why Option A Works** | The synergy between the pieces, in plain language |
| 4 | **How to Play It** | 3 to 5 concrete in-game tips |
| 5 | **Build Tradeoffs** | A table comparing A, B and C (for example ease of use, damage, survivability) |
| 6 | **Source Confidence** | High, Medium or Low, with the reason |

The skill uses each game's own words (Star Powers in Brawl Stars, Artifacts in Genshin Impact) and adapts the layout for games without classic builds, like weapon loadouts in PUBG Mobile or decks in Clash Royale.

## How it works

1. **Parse and check.** Work out the game type, pull out the character and any extra context, and ask if anything is unclear.
2. **Search.** Run at least 3 searches across the game's subreddit, a tier list or wiki, recent YouTube guides and the official Discord, using the per-game sources in `references/search-sources.md`.
3. **Quality gate.** Check that results are recent (ideally under 3 to 6 months old), that at least two independent sources agree, and that the answer holds up. If not, search again and flag the uncertainty.
4. **Build three options.** Meta, situational, and off-meta, each with every slot filled in.
5. **Deliver.** Format the reply exactly as in `references/output-template.md`.

If no real build data turns up, the skill says so and points to community resources instead of making one up.

## Games covered

Brawl Stars, Mobile Legends: Bang Bang, Honor of Kings, Wild Rift, Clash Royale, Genshin Impact, PUBG Mobile, Free Fire, AFK Arena and AFK Journey, Raid: Shadow Legends, and Summoners War. For any other game it falls back to a general search strategy and says so in the confidence line.

## Requirements

An agent that can search the web. No API keys, accounts or logins are needed.

## Install

**Claude Code and compatible agents.** Clone the repo straight into your skills folder:

```bash
git clone https://github.com/Umang1617/loadout-lab.git ~/.claude/skills/loadout-lab
```

**Other agents.** Copy `SKILL.md` and the `references/` folder into a folder named `loadout-lab` inside wherever your agent loads skills from, or upload the packaged skill file where skill uploads are supported.

## Make it yours

| To change | Edit |
|---|---|
| Games, subreddits, tier list sites, source ranking, red flags | `references/search-sources.md` |
| Layout and wording of the final answer | `references/output-template.md` |
| Steps, quality gate, edge cases and trigger phrases | `SKILL.md` |

## Repo contents

```
loadout-lab/
├── SKILL.md                      # the skill: trigger, 5 steps, quality gate, edge cases
├── references/
│   ├── search-sources.md         # where to look, per game, and how to rank sources
│   └── output-template.md        # the exact response format
├── README.md
├── LICENSE
└── .gitignore
```

## Good to know

- **Metas change fast.** Many mobile games patch every few weeks. Check the Source Confidence line, and cross-check on the game's subreddit or Discord before an important match.
- **Community advice can be wrong.** The skill gathers what players recommend, it doesn't test it.
- **Web content is untrusted.** Posts, videos and pages are read as-is, so use your judgment.
- **Never share personal details or log in through the agent.** Loadout Lab only needs a game and a character.
- **Third-party names.** Game, website and platform names in this project belong to their owners. This project is not affiliated with or endorsed by any of them. Check each site's terms of use before using this skill at scale.

## Roadmap

- [ ] Add more games to the sources list
- [ ] Optional patch-notes check
- [ ] Team-composition and draft advice for ranked play

## Credits

Built by [Umang Srivastava](https://www.linkedin.com/in/umang1617/).

## License

[MIT](LICENSE)
