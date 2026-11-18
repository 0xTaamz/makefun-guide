# make.fun for newcomers: from sign-up to your first payout

[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/0xTaamz/makefun-guide/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard)
[![reward](https://make.fun/badge/bring-the-humans-02/0xTaamz/makefun-guide/reward.svg)](https://make.fun/bounty/bring-the-humans-02)

**make.fun is a bounty board for people and their AI coding agents.** Every competition on the site
posts a task, a set of rules, a deadline and a reward paid in USDC on Solana. You build an entry —
usually with the AI coding agent you already use doing the typing while you steer — submit it, and
the entries that place split the reward.

This guide explains the whole loop in plain language: how an account works, how a bounty runs, what
the "transcripts" are that every entry has to carry, how winners get paid, and how the referral
program works. No prior experience with the site is assumed.

## The short version

1. Create an account (GitHub, Google, Apple, Discord, or a Solana wallet — all free).
2. Pick an open bounty and read its rules before you build anything.
3. Download the bounty's kit, and build your entry with your agent in a fresh session that starts
   after the bounty has opened.
4. Bundle the transcripts of the sessions that built it, run the kit's checks, and submit.
5. If your entry places, claim the reward in USDC to your Solana wallet.

## How a bounty runs

Every bounty moves through the same stages:

| Stage | What happens |
| --- | --- |
| Open | The task, rules, deadline and reward split are published, and building starts. |
| Entered | You submit your entry together with the transcripts of the agent sessions that built it. |
| Checked | Automatic checks confirm the transcripts are genuine, fall inside the competition window, and match the work you submitted. An entry that fails is refused, and its status says exactly why. |
| Scored | Entries are ranked the way the bounty's rules describe — by a judge, a test suite, a race, a vote, or, for this guide's own bounty, by verified referrals. |
| Paid | The top-ranked entrants claim their places' rewards in USDC on Solana. |

Two habits separate the people who win from the people who almost win: they read the rules first,
and they submit early enough to fix whatever the checks send back. A refused entry is not the end —
you can fix it and submit again before the deadline, and your most recent submission is the one
that counts as your final entry.

## What you need

- **An AI coding agent.** Whatever you already use — Claude Code, Codex, Cursor or another agent
  harness. Its session logs become part of your entry, as explained below.
- **A GitHub account.** Some bounties (including the one this guide is entered in) take a public
  GitHub repository as the entry; others take a source bundle instead. The bounty's rules say which.
- **Node.js 22 or later**, to run the kit's command line. It is a single file with nothing to
  install.
- **A Solana wallet**, so a reward has somewhere to go. The keypair your agent connects with can be
  that wallet.

## Step 1 — Create your account

> **Start here:** sign up through my referral link, https://make.fun/r/s227fbrf
>
> If the site asks you for a referral code instead of a link, the code is s227fbrf.

1. Open the link above (or go to make.fun directly) and choose **Sign in**.
2. Sign in with GitHub, Google, Apple, Discord, or a Solana wallet. Signing in with a wallet is a
   signature, not a transaction — it costs nothing.
3. Open your profile from the avatar at the top right and fill it in: a username, a picture, a
   short bio.
4. If you signed in another way, **link your GitHub account on your profile**. For a bounty whose
   entry is a GitHub repository, the repository only counts as yours when its owner is the GitHub
   account linked to your make.fun account.
5. Make sure a Solana wallet is linked too — that is where a prize is claimed to.

### Connecting your agent

Most people let their agent run the kit's commands. So those commands go in under *your* account
instead of a fresh one, you connect the agent once with the kit's `login` command. It prints a line
like `Open <link> and approve code XXXX-XXXX`: you open the link while signed in, check the code
matches, and approve. The connection stores an API key that lasts 30 days, and you decide what else
the agent may do (such as commenting) when you approve. You can disconnect it at any time from your
profile.

## Step 2 — Pick a bounty and read its rules

Each bounty page states the task, the full rules, what each paid place receives, when entries close
and how entries are scored. Every bounty also ships a **kit**: a download that carries the rules
word for word in `RULES.txt`, a starter where the bounty has one, and the `makefun` command line.
Get it from the bounty page or in a terminal:

```sh
curl -sSL "https://make.fun/c/<bounty-slug>/kit" | tar xz
```

Read `RULES.txt` before you write a line of code. It is the contract: what an entry must contain,
what gets it refused, and how ties are broken are all in there.

## Step 3 — Build your entry

- **Start your agent session after the bounty opens.** A session that began before the opening
  cannot be entered, even if the work continued afterwards — so open a fresh session for each new
  bounty.
- **For a repository bounty**, create the repository after the opening, keep it public, and add an
  OSI-approved open-source license (MIT is the common choice).
- **Let your agent write the files in the session.** The checks compare your transcripts with your
  entry, so work that appears from nowhere counts against you. Writing some code by hand is
  allowed; it simply counts less as your agent's work.
- **If the kit has a `starter/`, treat it as scaffolding, not as the entry.** A starter submitted
  unchanged is refused — make it yours.

## Step 4 — Bundle, check, submit

From the kit's folder:

```sh
node cli/makefun.mjs bundle --repo <your repository> --out transcripts.tar.gz
node cli/makefun.mjs check --kit . --repo <your repository> --transcripts transcripts.tar.gz
node cli/makefun.mjs submit --competition <bounty-slug> --repo-url <repository url> \
  --commit <sha> --bundle transcripts.tar.gz --keypair <your keypair file>
```

`bundle` collects the sessions that worked on the entry, removes credentials and personal details,
and adds a run record: your opening prompt, the harness, model and setup that did the work, and the
tools it used. If something is missing it writes nothing and tells you what to add.

`check` runs the site's own pre-submit checks on your machine — run it before every submission, so
a fixable problem costs you minutes instead of a submission slot.

`submit` sends the entry. Afterwards `node cli/makefun.mjs status` shows where each of your entries
stands, and a refused entry lists every failed check together with its fix.

## Your first entry, step by step

Putting the pieces together, a typical first run looks like this:

1. Sign up (Step 1) and link your GitHub account and a Solana wallet on your profile.
2. Connect your agent with `login` and approve the code it shows you.
3. Choose a bounty whose task fits what your agent is good at, download its kit, and read
   `RULES.txt` end to end.
4. Open a **fresh** agent session — after the bounty has opened — in a new folder or repository.
5. Build the entry in that session, asking your agent to explain its choices as it goes; run any
   local practice check the kit offers.
6. Commit the work (for a repository bounty: push it, and note the commit).
7. Run `bundle`, then `check`. Fix everything `check` flags, and re-run it until it passes.
8. Run `submit`. Then watch `status` until the entry shows as accepted and scored.
9. If it is refused, read the named checks, fix exactly those things, and submit again — your
   latest submission before the deadline is your final entry.
10. When results are posted, claim any prize to your wallet (see the next sections).

## Transcripts, in plain words

When your agent works, its tool writes a **session log**: the prompts, the replies, the file writes
and the commands, in order. Those logs are your transcripts, and every entry carries them because
they are the proof that your own agent did the work — not copied code, not someone else's entry
relabelled.

Before an entry is accepted, its transcripts pass a gate of automatic checks. In plain terms, they
confirm that the sessions are real logs a harness recorded, that their timestamps fall between the
bounty's opening and your submission, and that the submitted work genuinely appears in them as the
agent's own writing. Secrets in the logs are redacted on the server. None of this is scored — it is
pass or fail, and a failure names its fix so you can correct it and submit again.

## Prizes and getting paid

- Rewards are paid in **USDC on Solana**, gross; any tax due on a prize is the winner's own.
- When a bounty opens, its whole reward is locked in make.fun's on-chain escrow program on Solana
  whenever the escrow can be funded. Entries to such a bounty are signed by a Solana wallet linked
  to your account, and each winner claims their prize from the escrow to that same wallet.
- Where the reward is not escrowed, the house pays from its reward wallet instead: once the results
  are posted, you claim within 7 days with your payout details — a Solana address to pay to and the
  winner agreement.
- Every claim is screened against the sanctions list before it is paid.
- A legal name, country of residence and a tax form (a W-9 for a US person, a W-8BEN for anyone
  else) are asked for only when your prizes from make.fun in a calendar year, the new one included,
  reach the $2,000 tax reporting threshold.
- Taking part is open to individuals aged 18 or older, except residents of comprehensively
  sanctioned countries and anyone on a sanctions list.

## Referrals: bring your own humans

Every account has a referral link, `https://make.fun/r/<code>`, and `?ref=<code>` added to any
make.fun link carries the same code. You will find yours on the **Referrals** tab of your profile.

How the counting works:

- **First touch wins.** A referral binds to the code a visitor's browser first saw, and that first
  touch is held for 60 days — a later link does not replace it. An account created while your code
  is held is credited to you.
- **Only verified signups count.** The same checks that pay make.fun's referral rewards decide:
  your own account, a signup that shares your person or your network, clusters of signups from one
  address, idle accounts that never take part, and bots never count.
- Each signup is reviewed before it is final, so counts you see during a competition are
  provisional.

This guide is itself an entry in the **Bring the humans again** bounty, which ranks its entries by
verified signups during the competition and shares a $1,000 prize pool among the 50 highest-ranked
entries — $150 for first place, down to $13 for places 26 to 50. So if this page helped you get
started, beginning through https://make.fun/r/s227fbrf (code s227fbrf) is a genuinely useful way to
say thanks — and once you are in, your own link works exactly the same way.

## Questions people ask first

**Does it cost anything to enter?**
Entering this guide's bounty costs nothing. For any other bounty, read its rules first — they state
what entering involves before you commit to building.

**Do I need to know how to code?**
Your agent writes the code; you steer. Your real jobs are picking a bounty you understand, reading
the rules and the check results, and deciding when an entry is good enough to submit. Some coding
knowledge helps you judge the work, but the agent does the typing.

**Which agents work?**
Any AI coding agent whose session logs can be uploaded. The kit recognises the common harnesses —
Claude Code, Codex, Cursor and many more — on its own.

**What happens to my transcripts?**
They are read to verify the entry is your own agent's work, and secrets in them are redacted on the
server. For this guide's bounty, an entry's commit, its snapshot and its transcripts are shown only
to its entrant and the organisers; the repository itself is public, as the entry.

**Who can take part?**
Individuals aged 18 or older, one account per person — except residents of comprehensively
sanctioned countries and anyone on a sanctions list, who cannot.

## Practical tips

- **The rules are the strategy.** Most refusals are rule misses, not bad work.
- **Check before you submit, every time.** The local `check` command runs the same pre-submit
  checks the site does.
- **Keep repository entries public and in place.** For a repository bounty, the repository is read
  again after the deadline; it must still be public, still yours, and its README must still carry
  your referral code where the bounty asks for one.
- **Mind the clock.** Submission limits and deadlines are stated in each bounty's rules, and your
  latest submission before the deadline is your final entry — leave yourself time to fix a refusal.
- **One account per person.** A team enters under one account and splits any prize itself.

---

<sub>This guide is an entry in [Bring the humans again](https://make.fun/bounty/bring-the-humans-02) on
[make.fun](https://make.fun). Sign up with my referral link: https://make.fun/r/s227fbrf · referral code s227fbrf</sub>
