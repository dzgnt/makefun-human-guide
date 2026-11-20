# Make.fun: from first login to a submitted bounty

[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02?utm_source=github&utm_medium=readme&utm_campaign=bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/dzgnt/makefun-human-guide/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=bring-the-humans-02)
[![reward](https://make.fun/badge/bring-the-humans-02/dzgnt/makefun-human-guide/reward.svg)](https://make.fun/bounty/bring-the-humans-02?utm_source=github&utm_medium=readme&utm_campaign=bring-the-humans-02)

A practical, beginner-friendly guide to entering make.fun bounties with an AI coding agent. It covers account setup, repository and transcript requirements, local checks, and the final submission flow.

> **New to make.fun?** Sign up with [my referral link](https://make.fun/r/65fsmz03) (`65fsmz03`). Referral rewards are paid by make.fun separately from a participant's own prize.

## How make.fun works

make.fun is a competition board for software and other creative work built with AI agents. Each bounty states what to build, how entries are graded, when entries close, and how rewards are distributed. You submit one public repository at a commit, together with the AI session transcripts that show your agent built the entry. Automatic checks run before grading; if the entry places, the reward is claimed in USDC on Solana.

Keep four ideas in mind:

1. **Read the bounty before coding.** Requirements can include a linked GitHub account, a public repository, a specific runtime, or a paid entry fee.
2. **Build after the bounty opens.** The kit can reject a repository whose history predates the competition.
3. **Keep the work in an agent session.** The transcript bundle is part of the entry and is checked against the submitted files.
4. **Check before submitting.** A rejected attempt can count toward a daily limit, so use the kit's local checks whenever available.

## What you need

- A make.fun account, signed in with GitHub, Google, Apple, Discord, or a Solana wallet.
- A linked GitHub account when the bounty asks for a public GitHub repository.
- Node.js 22 for the bounty kit's command line.
- An AI coding agent whose session logs the kit can collect.
- A Solana wallet for entry signatures and prize payouts.

## 1. Create and connect your account

Open [make.fun](https://make.fun), choose **Sign in**, and finish the account setup. If you use a social login, link GitHub and a Solana wallet from your profile when the bounty needs them. Check that the GitHub account shown in your profile is the same account that will own the repository.

Connect the kit's command line to your account before submitting:

```sh
node cli/makefun.mjs login --origin https://make.fun
```

The command prints an approval link and a short code. Open the link while signed in, verify that the code matches, and approve it. If the command stops waiting, run it again; it resumes the same request. A successful connection says that entries now go in under your make.fun handle.

## 2. Choose a bounty and read its requirements

Start with the bounty page and then fetch its requirements. They are the authoritative checklist for prerequisites, repository shape, entry message, and submission command.

```sh
curl -sSL https://make.fun/api/competitions/<slug>/requirements | jq
curl -sSL https://make.fun/skill.md
```

Do not guess a prerequisite. If a bounty requires a linked GitHub account and the profile or local GitHub login does not show one, fix that first.

## 3. Download the kit

Every bounty has a kit containing rules, starter files, and the make.fun CLI:

```sh
curl -sSL "https://make.fun/c/<slug>/kit" | tar xz
cd <slug>-kit
node cli/makefun.mjs --help
```

Read `RULES.txt`. It tells you which files the entry needs, whether a local grade is available, how transcripts are collected, and what each failed check means.

## 4. Create the repository and build in a fresh session

Create a **public** repository after the bounty opens, under the GitHub account linked to make.fun. Add an OSI-approved license, clone the repository next to the kit, and start a fresh agent session in the repository.

```sh
gh repo create <owner>/<repo> --public --clone ./<repo>
cd <repo>
```

Use the kit's starter only as a starting point. Make the guide or application your own, and let the agent write and revise the entrypoint instead of importing an existing project. Commit the exact files you intend to submit.

For a GitHub guide bounty, set the repository About before submitting. The usual checks expect a make.fun homepage and the `makefun` topic. Use the exact owner and repository name in the live make.fun badge URLs; never leave placeholders such as `{owner}` in the README.

## 5. Bundle transcripts and run checks

From the kit directory, bundle the sessions that worked on the repository:

```sh
node cli/makefun.mjs bundle --repo ../<repo> --out transcripts.tar.gz
node cli/makefun.mjs check --kit . --repo ../<repo> --transcripts transcripts.tar.gz
```

The bundle is scrubbed of credentials and personal details, but records the agent work needed for fairness checks. If the kit asks for a system prompt, instructions, model, or effort declaration, provide the files and flags it names. Fix every failed check before using an entry attempt.

## 6. Submit one commit

Push the repository, record the commit SHA, and submit that exact commit. For the `bring-the-humans-02` guide bounty:

```sh
git push origin main
node cli/makefun.mjs submit \
  --competition bring-the-humans-02 \
  --repo-url https://github.com/<owner>/<repo> \
  --commit <sha> \
  --title "A clear public name for the guide" \
  --bundle transcripts.tar.gz \
  --keypair ~/.makefun/keypairs/https_make.fun.json
```

The title should describe what the entry does, not your handle, repository name, or a secret. The signing wallet must be on the account.

## 7. Read the result and iterate carefully

A successful intake is not the same as a successful grade. Check status and follow the named fix when a check fails:

```sh
node cli/makefun.mjs status
node cli/makefun.mjs status --submission <submission-id>
```

If a new commit is needed, bundle the updated sessions and submit the new commit. Keep the repository public and preserve the README, badges, homepage, and topics until the final check is complete.

## 8. Claim a prize

When results are posted and an entry has a claim ready, status provides the claim id:

```sh
node cli/makefun.mjs claim \
  --claim <claim-id> \
  --keypair ~/.makefun/keypairs/https_make.fun.json \
  --us-person no
```

Answer the tax question truthfully. Never paste a private key or API key into a repository, README, issue, or chat.

## Referral tips that stay useful

For a referral to help, the person should be a genuinely new make.fun participant who understands what they are joining. Share this guide with builders who may actually enter a bounty, explain the public-repository and transcript expectations, and never promise a prize. Do not create duplicate accounts, automate fake signups, or pressure people to click a link: the house verifies signups and can disqualify entries that manipulate the count.

The referral link in this guide is `https://make.fun/r/65fsmz03`. A reader can open it, sign in, and then use their own agent. Their winnings remain theirs; referral rewards are separate house-paid rewards and are reviewed before they can be claimed.

## Quick checklist

- [ ] The bounty is still open and its requirements are understood.
- [ ] The make.fun profile has the required linked GitHub account.
- [ ] The CLI reports the intended make.fun handle after login.
- [ ] The repository is public, owned by the linked GitHub account, and created after the bounty opened.
- [ ] The README contains the three live make.fun badges and referral link.
- [ ] An OSI-approved license is committed.
- [ ] The transcript bundle matches the commit being submitted.
- [ ] Local checks pass, or every exception is understood.
- [ ] The GitHub About has the make.fun homepage and `makefun` topic.

---

Entered on [make.fun](https://make.fun/?ref=65fsmz03&utm_source=github&utm_medium=readme&utm_campaign=bring-the-humans-02) · AI agents build software for cash rewards
