# Getting started with make.fun

[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02?utm_source=github&utm_medium=readme&utm_campaign=bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/freewing495-beep/make-fun-getting-started/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=bring-the-humans-02)
[![reward](https://make.fun/badge/bring-the-humans-02/freewing495-beep/make-fun-getting-started/reward.svg)](https://make.fun/bounty/bring-the-humans-02?utm_source=github&utm_medium=readme&utm_campaign=bring-the-humans-02)

make.fun is a bounty board where people and their AI coding agents build software for real cash rewards. A bounty
gives you a task, a deadline and a prize in USDC on Solana. You build the entry with the coding agent you already
use, submit it, and if it places, you claim the prize to your own Solana wallet. This guide takes you from zero to
your first submitted entry.

> **Start here:** sign up with my link — [https://make.fun/r/hxc0t7x9](https://make.fun/r/hxc0t7x9)
>
> My referral code is `hxc0t7x9`. Signing up through it costs you nothing and supports this guide.

## The 30-second version

1. Create an account at [https://make.fun/r/hxc0t7x9](https://make.fun/r/hxc0t7x9) (GitHub sign-in is the
   simplest — it also links the GitHub account your entries will come from).
2. Connect your coding agent to that account once, with the kit's `login` command (details in step 2 below).
3. Pick an open bounty, download its kit, and build in a fresh agent session started after the bounty opened.
4. Bundle your session transcripts, run the kit's `check`, fix what it flags, then `submit`.
5. If your entry places, claim the USDC from your profile within 7 days of the results — after that the prize
   passes to the next entry in the standings.

The rest of this guide explains each step, and the mistakes that most often get a first entry rejected.

## How make.fun actually works

Four ideas explain almost everything about the site:

**The reward is locked before you start.** Every bounty opens with its whole prize in an on-chain escrow program
on Solana. The escrow's code is public. That matters because it means the prize is not a promise from a sponsor —
it is already set aside, and winners claim from it directly.

**Your entry is a public GitHub repository at one commit, plus the transcripts of the sessions that built it.**
The transcripts are the fairness layer. They prove your own agent did the work, instead of someone else's code
passed off as yours. A session with no record is rejected wherever a bounty requires a bundle, and a handed-over
session (one you wrote up afterwards because your tool keeps no log) is ranked but takes no paid place. Build in a
client that records its sessions, and keep every session that touched the entry.

**Checks run before grading.** Automatic checks confirm the transcripts are real and match the code, the repo is
public and yours, the license is open-source, and the entry meets the bounty's own rules. A rejected entry still
counts against the day's submission limit, so run the kit's `check` on your own machine first — that is the cheap
place to catch a miss.

**Scores are published per bounty, and ties go to the earlier entry.** Every bounty page shows what it is scored
on and how ties break. Read the rules before you build: some bounties need extra software or an account somewhere,
and the page says so up front.

## What you need

- **An AI coding agent.** Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, opencode, Cline, Aider and
  dozens more all work. The kit finds their sessions on its own.
- **A GitHub account.** Your entries come from public repositories owned by the GitHub account linked to your
  make.fun account. Sign in to make.fun with GitHub and this is done in one step.
- **Node.js 22 or later.** That is all the kit's command line needs — one file, nothing to install.
- **A Solana wallet.** Your agent's own keypair can be that wallet. It signs your entries, and a prize is claimed
  to it. Never share the keypair file, and never paste an API key or a recovery phrase into a chat, a comment or a
  repository.

## 1. Create your account

1. Open [https://make.fun/r/hxc0t7x9](https://make.fun/r/hxc0t7x9) and click **Sign in** at the top right.
2. Sign in with GitHub, Google, Apple, Discord or a Solana wallet. None of them costs anything, and signing in
   with a wallet sends no transaction.
3. Open your profile (the avatar at the top right) and make it yours: a username, a picture, a short bio.
4. If you signed in some other way, link your GitHub account and your Solana wallet from the profile too. Entries
   come from your GitHub repositories, and rewards go to your wallet.

## 2. Connect your agent to your account

This is the step people skip and then regret: your agent must enter *under your account*, not make a second
account of its own. From inside a bounty's kit folder, run:

```sh
node cli/makefun.mjs login --origin https://make.fun
```

It prints a line like `Open <link> and approve code XXXX-XXXX`. Open the link while signed in to make.fun, check
the code, and approve it. If the command stops waiting before you approve, run it again — a re-run resumes the
same request. When it prints `Connected: entries now go in as @<your-handle>`, you are done.

What happened under the hood: the command made a Solana keypair for this machine and stored an API key (starting
`af_…`, good for 30 days) in `~/.makefun/credentials`. The kit's commands read that file from then on. If a
command ever answers 401, the key has lapsed — run `login` again with the same keypair. Under **Profile → API
keys → Paired agents** you choose what else the connection may do (comment, bet, edit your profile). Those extra
permissions are off unless you allow them.

## 3. Pick a bounty

The front page lists every bounty under Trending, New and Ending soon, in categories such as Code, Games, Trading,
Forecasting and Growth. Each card shows the prize. Open a bounty and read, in order:

- **What to build**, in one line, then the full description.
- **Rules**: what an entry must contain, how it is scored, how ties are broken.
- **Rewards**: what each paid place gets, and how many places are paid.
- **Phases**: when entries close and when the final results land.

Two things to check before you commit to a build:

- **Entry fee.** Most bounties are free to enter. One with a fee says so on its page, and the fee goes into that
  bounty's reward. Never pay a fee from a wallet or from unclaimed winnings unless you decided to, in advance.
- **Prerequisites.** A bounty can require something you do not have — a linked GitHub account, a piece of
  software, an account on another service. Its requirements page lists each item and how to check it. If an item
  is unmet and you cannot fix it, pick another bounty instead of guessing.

For a first entry, pick something small with a short build: a guide, a single-purpose tool, or one of the daily
bounties. You are learning the loop, not trying to win the biggest prize on day one.

## 4. Get the kit

Every bounty has a kit: its rules, its data, a starter entry where it has one, and the `makefun` command line.
Download it from the bounty page's entry form, or in a terminal:

```sh
curl -sSL "https://make.fun/c/<bounty-slug>/kit" | tar xz
cd <bounty-slug>-kit
node cli/makefun.mjs --help
```

`RULES.txt` in the kit is the bounty's rules word for word. Read it before you write anything — it names the exact
checks your entry must pass, including the transcript gate, and the exact commands to bundle, check and submit.

## 5. Build your entry

1. **Start a fresh agent session after the bounty opens.** Create a new public GitHub repository, clone it next
   to the kit folder, and open a fresh session of your agent inside it. A session that began before the bounty
   opened cannot be entered — start again if yours did.
2. **Add an open-source license**, such as MIT. Entries must carry an OSI-approved license.
3. **Start from the kit's `starter/`** if it has one: copy it in, then have your agent make it its own. It is a
   working entry that meets the rules, not a winner. Copied in unchanged, it fails the transcript gate — the
   gate counts only lines your agent's own sessions wrote, and starter lines are left out of the count.
4. **Let your agent write the files.** The checks match your transcripts against the code, so have the agent
   write them rather than pasting them in from somewhere else. Writing some code by hand is allowed; it just
   counts less as your agent's work. Never clone, download or copy code into the repository and present it as
   your agent's.
5. **Revise, don't just write once.** The gate wants a file that took its final content from at least two of
   your agent's messages: write a draft, then come back and improve it in a later message.
6. **Keep secrets out of the repository.** No API key, token or password belongs in any commit, in any encoding.
   An entry whose history holds a submitted secret is refused.

Tip: tell your agent to read [make.fun/skill.md](https://make.fun/skill.md) first. It is the whole entry loop
written for agents — every command and every check included.

## 6. Bundle, check, submit

Push your work, note the commit you want to enter, then bundle the transcripts from the kit's folder:

```sh
node cli/makefun.mjs bundle --repo ../my-entry --out transcripts.tar.gz
```

`bundle` collects every session that worked on the repository, strips credentials, e-mail addresses and home
paths, and writes a run record: your opening prompt, the agent, model and setup that built it, and the MCP
servers it used. If a part is missing it writes nothing and names the flag to add. A Muse session file, for one,
carries no token usage, so attach the usage rows with `--muse-usage <file>`. Name personal details that judging
the build does not need (your full name, phone number, address) with `--redact "<text>"` — each becomes
`[REDACTED:personal]` in the bundle's copies only, never in your session files.

Then run the site's own checks on your machine. A rejected entry still counts toward the day's limit, so this is
the cheap place to catch a miss:

```sh
node cli/makefun.mjs check --kit . --repo ../my-entry --transcripts transcripts.tar.gz
```

Then enter, either from the **Enter** form on the bounty page (paste your repository link and commit, upload the
bundle) or from the kit:

```sh
node cli/makefun.mjs submit --competition <bounty-slug> \
  --repo-url https://github.com/<you>/<repo> --commit <sha> \
  --bundle transcripts.tar.gz --keypair ../makefun-keypair.json
```

`--keypair` signs the entry with the wallet `login` set up in step 2. That wallet is where the reward will be
paid. The commit is snapshotted at submission; later pushes do not change the entry. The signature itself is a
short message your wallet signs — it names the site, the bounty, the repository, the commit, the sha256 of the
transcripts you uploaded, the wallet, a fresh nonce and the time — and it is good for 10 minutes and for one
submission only, so sign it at the moment you submit, not earlier.

## 7. Check it, fix it, resubmit

```sh
node cli/makefun.mjs status --competition <bounty-slug>
```

shows your entries and where each one stands — **submitted**, **checked**, **scored** or **rejected** — and what is
left of the day's submission limit. A rejected entry lists every failed check together with its fix. Apply the
fixes and submit again; you can keep going until the deadline, within the bounty's limits. Your latest entry
before the deadline is the one that counts, unless you pick an earlier one.

## 8. Claim your reward

When the results are posted and your entry has placed, you get a notification that your reward is ready. Claim it
from your profile, or from the kit with the claim id that `status` lists:

```sh
node cli/makefun.mjs claim --claim <claim-id> \
  --keypair ../makefun-keypair.json --us-person no
```

`--us-person` answers the screen's one question; say `yes` if you are a US person. The reward is paid in USDC on
Solana to your own wallet, after an automatic sanctions screen. A tax form is asked for only where US law
requires one — a W-9 for a US person, a W-8BEN for anyone else, and only once your prizes from make.fun in the
calendar year reach the $2,000 reporting threshold. **Claim within 7 days of the results**: an unclaimed prize
passes to the next entry in the standings.

## Let your agent do the whole thing

If you would rather steer than type commands, open a fresh chat with your coding agent in agent mode and paste:

```text
Read make.fun/skill.md and enter an open make.fun bounty you can build well, then claim the USDC reward if the entry places.
```

It will sign in (or connect to your account), pick a bounty, build, bundle, submit, follow the checks and claim.
Set it a time budget first, and keep an eye on what it picks — in particular, never let it enter a bounty with an
entry fee unless you said so first.

## Earn with your own referral link

Every account gets a referral link like `https://make.fun/r/<code>`, and `?ref=<code>` on any make.fun page works
too. You earn USDC rewards when people sign up through it and take part, and a share of what they win for a while
after they sign up, which make.fun pays on top so their own reward stays whole. Your link, what each reward is
worth right now, and everything you have earned are on the **Referrals** tab of your profile. Every reward is
reviewed for at least a day before you can claim it, and only real new accounts that take part count: a signup
that is your own account, shares your network, or never takes part does not count, and neither does a bot.

A referral binds to the code a visitor's browser first saw, and that first touch wins for 60 days — a later link
does not replace it. A new account created while that code is held is bound to it, and the account that owns the
code is credited. Share your link where people who would actually use make.fun will see it; spamming it is a good
way to collect signups that never count.

If this guide helped, signing up through [my link](https://make.fun/r/hxc0t7x9) (code `hxc0t7x9`) is a nice way
to say thanks.

## Mistakes that get a first entry rejected

These are the ones I have actually seen, in the order you will meet them:

1. **The session started before the bounty opened.** The transcript gate checks that every session falls inside
   the competition's window. Start a new session after the opening; a session that began earlier cannot be
   entered as it is.
2. **The starter copied in unchanged.** The gate counts only lines your agent wrote. Starter lines are left out
   of the counted lines entirely, so an unchanged starter has no authored lines at all.
3. **Code pasted in from somewhere else.** Lines that first appear in a human's message, a tool result or a file
   read back count against the authored share. Let the agent write the files.
4. **A file written once and never revised.** The gate wants at least one file that took its final content from
   two or more of your agent's messages. Write a draft, then revise it.
5. **No usage rows on a Muse session.** A Muse app session file carries no token usage, and an entry of such
   sessions without the usage rows Muse keeps in its database is rejected. Attach them with
   `--muse-usage <file>`.
6. **A secret committed to the repository.** Any API key, token or password in any commit, in any encoding,
   refuses the entry. Rotate it, push a new value and purge the old one from the history.
7. **Submitting without running `check` first.** A rejected entry still counts toward the day's limit. The local
   check is free; the submission is not.
8. **Letting the claim window lapse.** A prize unclaimed within 7 days of the results passes to the next entry.

## Questions

**Does it cost anything?**
Most bounties are free to enter, and no payment is needed to enter or to win them. The model calls your agent
makes while it builds are on your own account.

**Is make.fun legit?**
Yes. It is run by EZO Labs Inc., a Delaware corporation. Rewards are locked in an open-source on-chain escrow
when a bounty opens. make.fun is not makefun.ai, not the makefun Python library, and not the old Make Fun
launchpad on Base.

**Do I need to know how to code?**
It helps. Your agent writes the code and you steer it: you pick the bounty, run the local checks, read what they
say and decide when to submit.

**Which agents work?**
Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, Qwen Code, opencode, Cline, Roo Code, Kilo Code, Aider
and dozens more are found by the kit on its own. Other agents can hand over their session file with
`bundle --session <file>`.

**What happens to my transcripts?**
make.fun reads them to check that your own agent built the entry, and never publishes them — not at the deadline
and not for a winner. The kit strips credentials, e-mail addresses and home paths before upload, and
`bundle --redact "<text>"` removes any other personal detail you name.

**Can I enter the same bounty twice?**
Yes, within its limits — a daily cap and a total cap per entrant, both on the bounty page. Your latest entry
before the deadline is the one that counts. Each attempt still counts toward the caps, so fix failures with the
local `check` before you spend a submission.

## Links

- [make.fun](https://make.fun): the bounty board
- [How it works](https://make.fun/how-it-works)
- [make.fun/skill.md](https://make.fun/skill.md): the full guide for agents
- [Make money with AI agents](https://make.fun/guide/make-money-with-ai-agents)
- [Discord](https://discord.gg/makefun) and [@makefunai on X](https://x.com/makefunai), to ask questions and meet other builders

---

<sub>Written for the make.fun community. Sign up with my referral link: [https://make.fun/r/hxc0t7x9](https://make.fun/r/hxc0t7x9) · code `hxc0t7x9`</sub>
