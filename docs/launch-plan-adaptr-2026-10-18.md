# adaptr Show HN Launch Plan — 2026-10-18

**Owner:** Erlich (Angel Advisor / Launch Comms)
**Sprint:** Q3
**Status:** DRAFT — week 1 of Sprint Q
**Target launch date:** Sunday 2026-10-18
**Today:** 2026-04-28
**Weeks to launch:** ~25

This document is the WHEN/WHO/HOW around the adaptr Show HN post. It does NOT
duplicate Monica's Q2 deliverables (`docs/SHOW-HN-DRAFT.md`, comparison table,
FAQ pre-empts). It does NOT pre-empt Jared's Q4 audit. Where the pattern repeats
from the clustr plan (`staging/clustr/docs/launch-plan-2026-07-27.md`) or the
tunnl plan (`staging/tunnl.sh-docs/launch-plan-tunnl-2026-09-07.md`), this
document says so and does not re-explain it.

The clustr launch plan was written with 13 weeks of runway. The tunnl plan had 19.
This one has 25. The extra runway is not padding — it is a deliberate 6-week stagger
from tunnl (2026-09-07) so the founder can close out tunnl's HN thread fully before
the adaptr pre-launch push accelerates. It also makes adaptr the third in a visible
infrastructure tooling series, which changes the reception on HN from "solo project"
to "deliberate portfolio."

---

## 1. Posting Day and Time

### Primary slot

**Sunday 2026-10-18, 09:30–10:00 ET (06:30–07:00 PT)**

The Sunday 09:30 ET window is the same primary slot used for clustr and tunnl. The
reasoning is unchanged: US East Coast gets the first wave, West Coast follows, the
competitive post density on Sundays is lower than on weekdays, and a smaller initial
vote burst can hold a top-10 position. See `staging/clustr/docs/launch-plan-2026-07-27.md`
Section 1 for the full rationale.

October does not change this calculus. Mid-October is solidly post-Labor-Day
developer engagement territory — the September conference season (GopherCon,
ReactConf, KubeCon NA happens late October) is either finished or happening
elsewhere on the calendar. The HN audience is fully active and not distracted by
travel. KubeCon NA 2026 is likely scheduled for the week of October 26–30 (check
the actual date when it is announced). If KubeCon falls the same week as 2026-10-18,
shift to the backup slot or to the following Sunday — the Kubernetes audience is a
meaningful subset of adaptr's target persona and you do not want them on planes.

One October-specific dynamic that does NOT apply to the clustr or tunnl plans: the
US engineering community has a measurable Reddit and HN traffic dip the week of
US Thanksgiving (late November) and the weeks surrounding Christmas. Mid-October is
far enough from that dip to be unaffected. October 18 is the sweet spot — past the
September-to-October re-engagement peak, clear of any major conference conflicts.

The 6-week stagger from tunnl (2026-09-07) is intentional. Do not compress this
gap. The founder must be fully out of the tunnl HN thread before driving adaptr
pre-launch activity. A distracted founder in the adaptr thread is worse than a
delayed adaptr launch.

### Backup slot

**Monday 2026-10-19, 08:00–08:30 ET (05:00–05:30 PT)**

Same logic as the clustr and tunnl backup slots: serious HN readers check before
standup, velocity builds through the work morning. Monday risk is higher competition
density. Have three to four genuine community contacts ready to engage within the
first ten minutes if using the backup slot.

Trigger for backup: a critical bug surfaces Saturday night, a large competitor
(nginx project, Caddy, a new Cloudflare product) dominates HN Sunday morning, or
the founder is operationally unavailable.

A second backup consideration specific to adaptr: if KubeCon NA 2026 falls on the
weekend of October 18, shift to Sunday 2026-10-25 as the primary and Monday
2026-10-26 as the backup. The K8s ingress audience is a primary persona for adaptr.
Confirm the KubeCon 2026 NA date by July 2026 and lock the final posting date.

### A note on algorithmic discipline

Same standing rule as clustr and tunnl: do not solicit upvotes in any coordinated
way. The specific risk for adaptr is that the nginx and Kubernetes communities have
active Discord and Slack channels where a "check this out" message would land with
hundreds of people at once. Do not do this on launch morning. Plant seeds in the
pre-launch phase so engagement on launch day is organic. Mass Discord pings on
launch morning look like spam, violate HN norms, and can get the post flagged.

---

## 2. Pre-Launch (T-25 Weeks to T-1)

### Phase A: Foundation and parallel sprint work (T-25 to T-20, weeks 1–5)

This phase runs concurrently with clustr Sprint I finish and tunnl Sprint P work.
The adaptr Q-sprint deliverables are the main activity. Erlich is not blocking on
Q-sprint completion — this launch plan exists now.

**Week 1 (2026-04-28 to 2026-05-04)**

- [ ] Q3 launch plan (this document) authored and committed — Erlich
- [ ] Q2 deliverables confirmed complete: Monica's SHOW-HN-DRAFT.md and
  COMPARISON.md are committed at fa4b429. Review both for accuracy against
  v0.4.0 — do not let them go stale as the version advances.
- [ ] Q-Bug-1 (Next.js partial support verification) confirmed dispatched to
  Dinesh. This work must resolve before launch. Trust that it completes.
  If Dinesh's Q-Bug-1 work results in full Next.js support, Monica updates the
  limitations section of SHOW-HN-DRAFT.md to reflect it. If it confirms partial
  support as a hard constraint, the FAQ pre-empt #5 (Next.js) remains as-is.
- [ ] Identify GitHub topics to set on the repo before the launch window:
  `spa`, `reverse-proxy`, `subpath`, `kubernetes`, `helm`, `golang`,
  `self-hosted`, `docker`, `webpack`, `vite`, `react`, `vue`.

**Weeks 2–5 (2026-05-05 to 2026-05-31)**

- [ ] Confirm Q-sprint acceptance criteria green. No outstanding P1 issues.
- [ ] README: CI badge, license badge, Docker pull badge from GHCR.
- [ ] GitHub releases: confirm v0.4.0 release notes are readable by a stranger.
  The release page is the second thing an HN reader visits after the README.
- [ ] Erlich: identify 5–10 warm contacts across the SPA developer and sysadmin
  communities. Categories defined in the Community Targets section below. Draft
  outreach messages. DO NOT SEND yet — Phase C is the send window.

### Phase B: Community seeding and audience building (T-20 to T-12, weeks 6–13)

The 25-week runway makes a more thorough Phase B possible than either clustr or
tunnl had. Use it. This is the highest-leverage window for adaptr specifically
because the audience is broader than HPC (clustr) and less crowded than webhook
devs (tunnl). There is no "nginx is a household name" problem to overcome — there
is a "does this problem space have an open-source community around it?" question
to answer. Phase B builds that community.

**Week 6 (2026-06-02 to 2026-06-08) — Community mapping and dev.to article drafting**

Write the pre-launch dev.to article. Monica owns the draft. Working title:
"Deploying a React app behind a subpath without rebuilding it."

Frame it as a technical problem-and-solution post, not a product announcement.
The article earns organic search traffic on queries like "serve SPA at subpath",
"nginx sub_filter SPA", "React app subpath 404", "kubernetes ingress SPA basename".
These are the queries fired by the developer who just spent an afternoon on this
problem. The article should link to adaptr's repo and Docker image naturally — one
or two times, in context, not as a promotional close.

Publish no earlier than week 8. Let Q-Bug-1 resolve first so the article's
technical claims are fully accurate for the version it cites.

Identify where SPA developers spend time. These are not the same communities as
clustr's HPC sysadmins or tunnl's webhook devs. Do not apply the tunnl community
list to adaptr.

Primary adaptr developer communities:

- **r/reactjs** — the largest React community on Reddit. A post titled "I built a
  Go proxy that fixes the 'SPA behind subpath' problem at runtime — feedback
  welcome" belongs here. This audience has personally hit the problem. They are not
  the typical sysadmin who reads r/sysadmin; they are the frontend developer who
  deployed their app and found it broken at `/myapp/`.
- **r/vuejs** — same audience, Vue framing.
- **r/nextjs** — post only after Q-Bug-1 resolves and the Next.js story is clean.
  Do not seed r/nextjs with partial Next.js support claims.
- **r/Frontend and r/webdev** — broader frontend communities. A technical post with
  a working demo will be well received here.
- **r/sysadmin and r/devops** — the Kubernetes ingress angle. The target commenter
  here is the sysadmin who manages nginx ingress for a platform team and has had
  to hand-hold SPA teams through the `basename` problem for years.
- **Reactiflux Discord** — the largest React developer Discord. The `#help` channel
  surfaces people hitting the exact problem adaptr solves in real time. A
  well-timed response to one of those threads ("I actually built a tool for this —
  here is the repo") is more valuable than a cold announcement post.
- **Vue.js Discord and Vercel community Discord** — same principle.
- **Kubernetes Slack (#ingress-nginx channel)** — this is the highest-precision
  pre-launch channel for the K8s persona. The people in that channel have been
  bitten by the ingress subpath problem and will recognize the solution immediately.
- **Indie Hackers** — a post about building adaptr as an infrastructure tool in
  the SPA space, open-sourced, with a Docker container and a Helm chart, fits the
  IH builder narrative. Also useful for cross-linking to tunnl's IH presence if
  tunnl's IH post generated engagement.
- **dev.to** — the pre-launch article (see above) lives here. Tag it
  `#webdev #react #devops #kubernetes`.
- **Hashnode** — secondary content distribution for the dev.to article. Mirror
  it or publish a variant there. The Hashnode audience skews slightly more
  DevOps/infrastructure than dev.to.

Open-source SPAs to demonstrate compatibility — seed with these projects explicitly
before launch. These communities have the problem and can verify the solution:

- **Open OnDemand** (OSC's OOD) — the canonical dynamic-subpath case. The OOD
  community forum (discourse.openondemand.org) is the right channel. Frame adaptr
  as "a container that handles the `rnode` subpath problem for SPAs inside OOD."
- **Plausible Analytics** — the Plausible self-hosted community has discussed
  subpath deployment challenges. Their GitHub discussions and community forum are
  seeding targets.
- **Mastodon web UI** — the Mastodon instance admin community runs SPAs behind
  subpaths in some configurations. The #Mastodon admin community on Mastodon itself
  and in the Fosstodon community is a niche but relevant target.
- **Gitea / Forgejo frontend** — Gitea's SPA frontend has subpath deployment
  documentation that is known to have edge cases. The Gitea/Forgejo developer
  community is a good validation target.

**Week 7 (2026-06-09 to 2026-06-15) — clustr is in final hardening**

No new adaptr community pushes this week. clustr is in its final four-week code
freeze and the founder's attention is correctly on clustr. Adaptr pre-launch work
pauses. Erlich monitors the adaptr GitHub star count and issue queue passively.

**Week 8 (2026-06-16 to 2026-06-22) — dev.to article live**

- [ ] Monica's dev.to article published. Tag: `#webdev #react #devops #kubernetes`.
  Do not submit to HN yet. Submit to r/webdev with a technical framing.
- [ ] Submit adaptr to relevant awesome lists: `awesome-kubernetes`, `awesome-react`,
  `awesome-selfhosted`. These are permanent indexed references. PR title pattern:
  "Add adaptr — runtime SPA subpath adapter, no rebuild required."
- [ ] Founder begins authentic HN engagement in relevant threads: nginx subpath
  discussions, Kubernetes ingress threads, SPA deployment threads, Go open-source
  projects. This is not gaming HN. It is being a genuine participant with an
  account that has a comment history before the launch date.

**Week 9 (2026-06-23 to 2026-06-29) — clustr final week, no adaptr activity**

clustr launches 2026-07-27. This week is clustr's final preparation week. No
adaptr community pushes. Tunnel the available attention entirely into clustr.

**Weeks 10–12 (2026-06-30 to 2026-07-19) — pre-clustr-launch quiet period**

Same pattern as the tunnl plan: adaptr work shifts to passive maintenance. Answer
any GitHub issues, monitor the dev.to article for traction signals, do not start
new adaptr community pushes that dilute attention from clustr's final sprint.

**Week 13 (2026-07-20 to 2026-07-26) — clustr final week**

No new adaptr activities. clustr launches 2026-07-27.

**Week 14 (2026-07-28 to 2026-08-03) — clustr thread observation**

clustr HN thread is live. Same observation task as the tunnl plan: watch the
comment patterns, objection types, and opening-comment responses that worked.
Extract lessons applicable to adaptr. Specifically note:

- Did the compare-and-contrast ("why not just use X?") comment structure in
  the first hour consume most of the early thread oxygen? If so, adaptr's
  pre-emptive FAQ structure must be even tighter — the comparison table in
  `docs/COMPARISON.md` needs to be referenced in the opening comment by name,
  not just hoped to be found.
- What was the star-to-GHCR-pull ratio for clustr in the first 24 hours?
  Use this as a calibration baseline for adaptr's expected ratio.

**Weeks 15–17 (2026-08-04 to 2026-08-23) — tunnl final sprint and OOD seeding**

tunnl is in its final pre-launch sprint. Adaptr activity is limited but one
high-value action should happen in this window:

- [ ] Post to the Open OnDemand community (discourse.openondemand.org) a technical
  post specifically about the rnode subpath problem. Do NOT announce adaptr as
  a product. Post the problem framing: "Has anyone written a proxy-level solution
  for the SPA asset path problem in OOD sessions? I've been working on something."
  This seeds the OOD community before the HN launch and primes the most technically
  qualified reviewers. The OOD core maintainers are at OSC (Ohio Supercomputer
  Center) and are active in that forum. If they comment positively before HN
  launch day, that is social proof with the exact niche audience.

- [ ] Seed one open-source SPA community (Plausible or Gitea) with a low-key
  "I tried adaptr with your deployment docs, here is what worked" post. Real
  usage reports are the highest-credibility seeding available.

**Week 18 (2026-08-25 to 2026-08-31) — tunnl final smoke test week**

No new adaptr community pushes. Tunnl launches 2026-09-07.

**Week 19 (2026-09-07) — TUNNL LAUNCHES**

The founder is fully engaged in the tunnl HN thread. Adaptr pre-launch is paused.
No adaptr activity until the tunnl thread cools.

### Phase C: Post-tunnl pivot to adaptr (T-6 to T-1, weeks 20–24)

tunnl launches 2026-09-07. adaptr launches 2026-10-18. That is 6 weeks. Use them
exactly as the tunnl plan used the 6 post-clustr weeks.

**Week 20 (2026-09-08 to 2026-09-14) — tunnl thread observation + lessons**

Erlich produces a one-paragraph "lessons from tunnl HN thread" note by end of this
week. Feed it into the adaptr opening comment structure. Specifically note:

- Which comparison objection dominated the thread? For adaptr, the "just use Vite
  base" and "nginx sub_filter exists" comments will arrive in the first hour. Monica's
  FAQ has pre-empts for both. Confirm they are still accurate and tight.
- What opening comment structure earned the most positive engagement in the tunnl
  thread? Mirror it.
- What was the tunnl star/GHCR-pull ratio? Build adaptr's success thresholds
  proportionally.

By this point the founder has run two HN thread playbooks. The patterns are known.
The adaptr launch is the third — and the founder should feel genuinely practiced,
not anxious.

**Week 21 (2026-09-15 to 2026-09-21) — warm outreach begins**

- [ ] Erlich sends personal warm outreach messages to the 5–10 contacts identified
  in Phase A. Message format:
  "Hey — I've been working on something I think you'd have opinions about. It's a
  Go container that fixes SPA subpath deployment at the proxy level — no rebuild,
  no app changes. Works with Kubernetes ingress, Caddy, nginx. Would you take 5
  minutes to look before I post it on HN in a few weeks?"
  This is not a request to upvote. It is a request for feedback.
- [ ] Do NOT send to more than 10 people. 5–8 genuine contacts is ideal.
  Quality over quantity. The same discipline as clustr and tunnl.
- [ ] If the dev.to article is generating organic traffic (visible from dev.to
  analytics), note the top-performing search terms and confirm Monica's Show HN
  draft opening addresses those exact pain points.
- [ ] Cross-link adaptr as "the third in our infrastructure tooling series" in
  any warm outreach that already knows clustr or tunnl. The portfolio context is
  a trust signal that solo-project framing does not have.

**Week 22 (2026-09-22 to 2026-09-28) — warm outreach follow-up**

- [ ] Follow up on outreach responses. If a contact identifies a bug or friction
  point, fix it.
- [ ] Monica: final Show HN post copy reviewed. Founder reads it aloud. If any
  sentence sounds like marketing copy, cut it.
- [ ] Confirm Q-Bug-1 (Next.js) is resolved and SHOW-HN-DRAFT.md is current.
  This is the last safe moment to update the limitations section. After this week,
  only P0 fixes are permitted.
- [ ] Confirm Monday 2026-10-19 is cleared as backup slot.
- [ ] Check KubeCon NA 2026 date. If it falls on the week of 2026-10-18, shift to
  2026-10-25 and update this plan.

**Week 23 (2026-09-29 to 2026-10-05) — code freeze**

- [ ] Code freeze for adaptr. Only P0 bug fixes permitted after this point.
  No feature additions, no schema changes, no new Helm chart values.
- [ ] Confirm launch-day duty roster (Section 3).
- [ ] Erlich notifies 2–3 highest-quality contacts that the post goes up Sunday
  2026-10-18 morning. Not an ask — a heads-up.
- [ ] GitHub topics confirmed set.
- [ ] README hero and comparison table reviewed for accuracy against the release
  version (v0.4.x or whatever version is current at this point).

**Week 24 / T-1 (2026-10-06 to 2026-10-17)**

- [ ] Fresh-install test: founder runs the full Docker quick-start from the README
  on a clean VM. Not from memory. Not with shortcuts. Verbatim. Confirm that
  `docker run -e TARGET=... -e BASE_PATH=... ghcr.io/sqoia-dev/adaptr:latest`
  works against a real SPA in under 60 seconds as promised.
- [ ] Confirm Helm chart quick-start works end-to-end: `helm install` against a
  test cluster, SPA accessible at the configured subpath.
- [ ] Confirm GitHub repo is public, latest release artifacts are live, Docker image
  is current for the version cited in the post.
- [ ] Monica's Show HN draft is copy-pasted into a staging document, ready to post.
  Title, body, and opening comment are finalized. Nothing left to write on launch
  morning. The title is locked:
  `Show HN: adaptr – serve any SPA at any subpath without rebuilding it`
- [ ] Confirm the cross-link references to clustr and tunnl in the adaptr README
  or docs are accurate. If clustr or tunnl launched successfully, those GitHub
  star counts and HN thread links are useful social proof to reference in the
  README ("from the team that built clustr and tunnl").
- [ ] Founder gets adequate sleep Saturday night.

---

## Community Targets for Pre-Launch Warm Outreach

These are categories, not named individuals. The founder must identify specific
contacts from their own network or from genuine community participation. Do not
cold-outreach someone you have never interacted with.

**Target profile 1 — Frontend developers who have publicly complained about the SPA subpath problem**

These exist in r/reactjs, r/vuejs, r/webdev, on GitHub Issues of major SPA
frameworks (React Router, vue-router), and in Reactiflux Discord threads. A person
who posted "why does my React app break when deployed at /myapp/?" three months ago
is a warm outreach target if you have engaged with their thread genuinely. Their
problem is the exact problem adaptr solves. A message to them saying "I actually
built a tool for this — would you try it before I post it on HN?" is not a cold
pitch. It is a continuation of a conversation they started.

**Target profile 2 — Kubernetes / DevOps engineers who manage ingress for multiple SPAs**

Find them in the Kubernetes Slack (#ingress-nginx), in r/devops, in CNCF community
channels. These are the platform engineers who have written internal runbooks about
how SPA teams must configure their `basename` props. They know the problem from the
other side — they are the ones telling the frontend teams what to do. adaptr is a
sidecar that removes the conversation entirely. A message framed as "I built a sidecar
that handles the ingress-SPA-subpath coordination at the container level — would you
look at it?" will land well here.

**Target profile 3 — Open OnDemand community members and university HPC staff**

The OOD community (discourse.openondemand.org and the OSC Slack/email list) is a
direct overlap with clustr's community. If the clustr launch generated any
university HPC staff contacts, those same people are adaptr targets. They know the
rnode subpath problem personally. This is the only audience where the OOD angle
should be the primary framing. For all other communities, lead with the generic
SPA developer case.

**Target profile 4 — HN regulars in the Go / self-hosted / developer tools space**

Same approach as the clustr and tunnl plans: search Algolia HN for "SPA subpath",
"nginx sub_filter SPA", "React Router basename", "kubernetes ingress SPA". Find
recurring commenters with substantive histories. Reach out with a genuine feedback
ask, not an upvote ask.

**Target profile 5 — Reactiflux and Vue.js Discord regulars who answer deployment questions**

The people who answer deployment questions in Reactiflux's `#help` and `#q-and-a`
channels are often senior developers who have seen every SPA deployment failure mode.
If one of them has answered a "subpath deployment broken" question recently, they
are a warm outreach target. Their endorsement on HN launch day — even just a
comment saying "I've been recommending solutions like this for years" — carries
weight because they are recognizable as a community helper, not a shill.

**Hard limit: 5–10 warm contacts total.** Same discipline as clustr and tunnl.
Quality over quantity. An unsolicited message to a stranger asking them to look at
your repo is a cold outreach, not a warm intro — and it rarely works on HN-caliber
people.

---

## 3. Launch Day (T-0, Sunday 2026-10-18)

### Duty roster

| Role | Person | Responsibility |
|---|---|---|
| Post author | Founder | Submits the Show HN post at 09:30 ET. One-time action. |
| Comment responder | Founder | Primary voice in the HN thread. No one else comments in the thread under their own name. |
| Engineering standby | Dinesh | On-call for P0 bugs surfaced in comments. Has push access, can cut a patch release and update the Docker image. |
| Infra standby | Gilfoyle | Watches GHCR image pull availability, GitHub release artifact availability, Docker image tag currency. |
| GitHub issues triage | Jared | First response to new issues within 2 hours on launch day. Tag as bug/question/enhancement. |
| Community monitor | Jared | Watches r/reactjs, r/vuejs, r/webdev, r/sysadmin, and Kubernetes Slack for cross-posts or discussion. Does NOT cross-post without founder approval. |
| Social signal monitor | Monica | Watches HN comment score and tone, dev.to article traffic, developer communities. Reports traction signals to the group. |

### Opening comment

The opening comment (pre-written by Monica, Q2 sprint) must be posted within 5
minutes of the main Show HN post going live. This is specific to adaptr and
follows the tunnl plan's MCP-demo equivalent: give the audience something concrete
to verify immediately.

The opening comment structure:

1. **Founder here, happy to answer questions.** One sentence. No preamble.
2. **The three-layer problem framing.** Two sentences. Name asset paths, client-side
   routing, and runtime-constructed URLs explicitly. This signals that the builder
   understands the full failure surface, not just the first 404.
3. **The honest comparison.** One short paragraph. Name Vite `base` directly and
   say when it is the right answer. This pre-empts the "just use Vite base" comment
   from arriving unanswered in the first 30 minutes. See Risk 1 below.
4. **What is different from nginx sub_filter.** Two sentences. The three-word version:
   gzip, framework awareness, runtime interception. This pre-empts Risk 4.
5. **One sentence about the portfolio context.** "This is the third tool in our
   infrastructure series — the prior two (clustr for HPC provisioning, tunnl for
   webhook inspection) are linked in the README if that context is useful."
   This is unique to adaptr and should not be in the post body — it belongs in the
   opening comment where it reads as natural context, not marketing.

Monica owns the exact text of this comment. The structure above is the brief.

### First 30 minutes (09:30–10:00 ET)

Same pattern as clustr and tunnl:
1. Founder submits the Show HN post (copy pre-staged from Monica's Q2 draft).
2. Founder posts the opening comment within 5 minutes.
3. Pre-primed contacts notified in week 23 naturally check HN that morning.
4. Founder checks once at 09:45, once at 10:00. Does not refresh compulsively.

### First 4 hours (10:00–13:30 ET)

- Founder engages with every substantive technical comment. Short, honest replies.
- Specific to adaptr: the "just use Vite base" comment will arrive in the first
  hour. Monica's Q2 FAQ pre-empt is pre-written. Do not improvise on this one.
- The "nginx sub_filter exists" comment will arrive in the first hour. Same:
  use the pre-written FAQ answer verbatim or nearly verbatim.
- The Next.js question will arrive if Q-Bug-1 did not fully resolve. If Next.js
  support is still partial, the honest answer is: "Partial support — static export
  covers most patterns, but some image optimization and dynamic import paths need
  additional configuration. Documented in the repo. Working on a complete fix."
  Do not claim full Next.js support unless Dinesh's Q-Bug-1 work confirmed it.
- Dinesh monitors GitHub for new issues opened by readers who try the Docker
  quick-start. If a P0 bug surfaces (Docker run fails completely, BASE_PATH env
  var not picked up), Dinesh ships a hotfix immediately. Founder acknowledges in
  the thread: "Good catch, fix is in the latest image — thank you."
- Jared triages all new GitHub issues within 2 hours.
- Monitor for the post breaking the front page (top 30 on news.ycombinator.com).

### Comment-response rules

Same standing discipline as clustr and tunnl. The additional adaptr-specific rules:

Do not apologize for the OOD angle if it surfaces. The dynamic subpath case (OOD's
rnode paths) is a genuinely hard problem that illustrates the product's value at its
most extreme. If an HN commenter says "this looks like an HPC niche tool," the
answer is direct: "The OOD case is the hardest version of the problem — most users
are running vanilla React behind nginx ingress and the quick-start resolves it in
60 seconds. OOD is an example I mention because the path is dynamic at boot time,
which is the case no other tool handles."

Do not engage with Vite comparison comments as if they are attacks. They are not.
The correct response acknowledges Vite `base` as the right answer in its lane and
then states precisely when that lane ends. See Risk 1.

---

## 4. Post-Launch (T+1 Day to T+1 Month)

### Week 1 post-launch (2026-10-19 to 2026-10-25)

Same triage cadence as clustr and tunnl:

| Task | Owner | SLA |
|---|---|---|
| GitHub issue first response | Jared | Within 24 hours |
| Bug triage (P0/P1) | Dinesh + Jared | Same-day |
| P0 hotfix (Docker install failure) | Dinesh | Within 48 hours |
| PR review (external contributor) | Dinesh | Within 72 hours |
| HN thread follow-up | Founder | As needed, no SLA |
| Community cross-post engagement | Jared | Daily check |

Day 1–3: all new issues get a response.
Day 4–7: close duplicates, tag enhancements as Q4 or Sprint R candidates.
End of week 1: Erlich produces a one-paragraph summary of what the community
surfaced. This feeds directly into Q4 scoping and any Jared audit.

### Follow-up content (T+1 to T+4 weeks)

**Technical blog post — T+7 days:**

Write a post-launch technical post. Working title: "How adaptr rewrites SPA paths
at runtime without touching the app." Frame it as an implementation walkthrough —
the HTML parser, the framework detector, the JS rewriter, the gzip round-trip. This
post is for the HN reader who starred adaptr and wants to understand how it works.
It generates a second wave of HN traffic as a standard link post (not Show HN) and
provides long-tail search indexing on implementation-specific queries.

Submit to dev.to (link back to the pre-launch article), Hashnode, and HN as a
standard link post. The MCP deep-dive equivalent from tunnl's post-launch plan
applies here: a well-written implementation article in October 2026, when
infrastructure-as-a-container tooling is early, will have sustained search and
community life beyond the HN launch window.

**Conference CFPs:**

The adaptr post-launch conference landscape is different from both clustr and tunnl:

- **KubeCon NA 2026 (late October / early November):** If KubeCon falls within two
  weeks of the adaptr launch, the timing is useful for informal hallway conversations
  but the CFP will already be closed. Note the talk idea for KubeCon EU 2027
  (typically March/April): "Sidecar proxy for SPA subpath routing without rebuild."
  The Kubernetes SIG-Apps and SIG-Networking communities are the right audience.
- **FOSDEM 2027 (February, Brussels):** The Web and JavaScript DevRoom and the
  Go DevRoom are both appropriate targets. FOSDEM CFPs typically open in October.
  The launch timing is ideal — post HN engagement as a calling card, submit to
  FOSDEM in late October or November. A talk framed as "runtime SPA path rewriting
  in pure Go" fits the Go DevRoom precisely.
- **GopherCon 2026 (if CFP still open post-launch):** A talk about the Go
  implementation — HTML parser, streaming rewrite, gzip round-trip — is technically
  interesting independent of the product. Check GopherCon CFP status post-launch.
- **Nginx conf or similar:** If nginx-adjacent conferences have CFPs open, a talk
  comparing structured proxy-level rewriting to nginx sub_filter is a natural fit.

Jared owns CFP research and submission. Lead time is 4–8 weeks minimum; start
immediately post-launch for any open CFPs.

**Cross-linking the portfolio:**

Post-launch, update the adaptr README to explicitly reference clustr and tunnl.
The three-tool portfolio framing should be visible to anyone who discovers adaptr
through HN, awesome lists, or the dev.to article. A section like "More from Sqoia
Labs" with one-line descriptions and GitHub links compounds the credibility signal.
This is a standing action, not a time-sensitive one — complete it within the first
week post-launch.

Jared: schedule a Q4 first-run audit for adaptr's post-launch install path, using
the same methodology as clustr's I5 and tunnl's P4 audits. The Q4 audit date is
not pre-determined here — it should be scheduled by Jared based on the first-week
issue queue and the founder's bandwidth post-launch.

### Adoption metrics that matter

| Metric | Where | Target (T+1 week) | Target (T+1 month) |
|---|---|---|---|
| GitHub stars | repo | 150+ | 600+ |
| GitHub forks | repo | 15+ | 60+ |
| Issues opened (non-bug) | repo | 8+ | 25+ |
| GHCR image pulls | ghcr.io/sqoia-dev/adaptr | 100+ | 1000+ |
| Helm chart installs | ghcr.io/sqoia-dev/charts/adaptr | 20+ | 200+ |
| `README.md` / `docs/` page traffic | GitHub insights | top doc | top doc |
| dev.to article traffic | dev.to analytics | 1000+ views | 3000+ views |
| External blog mentions | search | 1+ | 5+ |

The star target (150+ in week 1) is higher than clustr (100+) and tunnl (150+)
because adaptr's total addressable HN audience — every SPA developer who has
deployed behind a reverse proxy — is the broadest of the three products.

The GHCR image pull target (100+ in week 1) is the most meaningful signal. It means
someone actually pulled the container. Track the star-to-pull ratio: a healthy ratio
is 1 GHCR pull per 5–10 stars. If the ratio is much lower (many stars, few pulls),
the install friction is too high for the audience. If the ratio is much higher
(many pulls per few stars), the product is finding adoption through a channel other
than HN — a good signal worth investigating.

The Helm chart install target (20+ in week 1) is a new metric not tracked in the
clustr or tunnl plans. Helm installs indicate K8s-native deployments. This is the
audience Monica flagged as a secondary HN persona. A healthy Helm install count
confirms that the K8s angle resonated.

### Sprint R scoping from community feedback

Same feedback loop as the clustr and tunnl plans: the HN thread and first week of
GitHub issues will surface a short list of what the community actually wants.
Before Sprint R is dispatched:
- Erlich produces a one-paragraph characterization of the feedback themes
- Richard classifies each theme by build priority (BUILD-NOW, LATER, SKIP)
- Sprint R scope is drafted from the BUILD-NOW items that community feedback elevated

The most likely Sprint R candidates based on Monica's pre-launch intel:
- Full Next.js support (if Q-Bug-1 left residual gaps)
- Multi-SPA mode (single container, multiple subpath configs)
- OTEL / observability hooks
- Brotli output support

---

## 5. Risks Specific to adaptr

### Risk 1 — "Just use Vite base" dominates the thread

**Probability: HIGH.** This will be the first or second comment in the thread.
Every experienced SPA developer's instinct is to fix subpath issues at the build
level. The comment will be framed as "you're solving the wrong problem."

**Response framework (Monica's Q2 FAQ has the exact text — use it verbatim):**

The correct response acknowledges Vite `base` as the right answer in its lane.
The three cases where it stops working are: (1) the path is not known at build time
(OOD's rnode paths), (2) there is no source access, (3) the CI pipeline cost
exceeds the subpath change. If none of those apply, Vite `base` is the right choice.
adaptr is for when it is not.

What you must NOT say: "adaptr is better than Vite base." It is not, in the cases
where Vite base is available. It is the only option in the cases where it is not.
The distinction matters on HN. Frame adaptr as additive, not competitive.

The opening comment pre-empts this by naming Vite `base` directly and stating when
it is the right answer. Pre-empting the comment is better than responding to it.

### Risk 2 — OOD framing makes adaptr look like an HPC niche tool

**Probability: MEDIUM.** The OOD use case is the most technically interesting and
the most extreme version of the problem (dynamic subpath per session). It is also
niche. If the HN post leads with OOD, the audience for every other SPA developer
narrows immediately to "HPC people only."

**Mitigation:**

Lead with the generic case in the post body: React app behind nginx, broken at
`/myapp/`. OOD appears only in the "What it does" section as one example of the
dynamic subpath case, not as the primary persona. Monica's Q2 draft already follows
this framing correctly.

In the opening comment, OOD can be mentioned in one sentence: "The dynamic subpath
case (Open OnDemand's rnode paths) is the extreme version — the path is different
per session so there is nothing to compile in at build time." One sentence.
Not a paragraph. Not the lead.

If a commenter says "this looks like an OOD tool," the response is: "OOD is the
hardest case. Most users are deploying React apps behind nginx ingress — the
quick-start resolves that in 60 seconds. OOD is the example that proves the tool
handles the most dynamic variant."

### Risk 3 — Next.js failure becomes the story

**Probability: DEPENDS ON Q-Bug-1.** If Dinesh's Q-Bug-1 work confirms partial
Next.js support remains a hard constraint, and if an HN commenter tries adaptr with
a Next.js app and reports a broken experience, that commenter's thread will dominate
early engagement.

**Mitigation:**

Trust Q-Bug-1 to resolve before launch. If it does, this risk is downgraded.
If it does not, the launch limitations section must name Next.js partial support
explicitly and quantify what works and what does not. Monica's current draft already
lists it as a limitation. Do not soften or remove that limitation to make the post
sound cleaner. HN will find it in the first hour.

If a commenter reports a Next.js failure during the thread, the response is:
"Confirmed. Next.js static export covers most patterns but some dynamic import and
image optimization paths need additional config. Documented in the repo. This is the
active work item." Do not be defensive. The limitation is not a product failure —
it is honest scoping.

Under no circumstances should the launch be blocked pending full Next.js support
if Q-Bug-1 cannot confirm it before the code freeze. A clean partial-support answer
is better than a delayed launch. The retry-signal outcome (Section 7) is preferable
to over-promising on Next.js and getting caught in the thread.

### Risk 4 — nginx sub_filter purist comments

**Probability: MEDIUM.** The sysadmin audience on HN includes nginx power users who
know sub_filter well and will argue that the limitations Monica describes (gzip,
framework awareness, runtime interception) are overstated or solvable with the right
nginx config.

**Response framework:**

Do not argue. Do not say nginx sub_filter cannot handle these cases. Say adaptr
handles them in a single container with no nginx config. The response is not
"sub_filter is bad." It is "sub_filter + gunzip + correct location blocks + correct
sub_filter_types + a custom JS injection script is a valid approach, and adaptr is
what that solution looks like as a container with a single env var. If you've already
built the nginx version, you don't need adaptr. If you haven't, the container is
faster."

The comparison table in `docs/COMPARISON.md` is the reference. If a commenter
makes a specific technical claim about sub_filter capabilities that is accurate,
acknowledge it and update the comparison table if warranted. Do not dig in on a
comparison table detail if the commenter is right. HN respects acknowledging
correct technical corrections.

### Risk 5 — AI-written code cynicism

**Probability: MEDIUM.** Same risk as clustr and tunnl. Every commit is authored as
NessieCanCode (per CLAUDE.md standing rule). On HN, this will surface as "is this
a vibe-coded toy?"

Same playbook as the prior two plans:

"The commit attribution is accurate — this project uses an AI-assisted development
workflow. The architecture decisions, product design, and positioning are human-
authored. The code is tested, CI-enforced, and you can read it. Judge it by whether
it works."

Do NOT claim the code is entirely human-written. Getting caught in that
misrepresentation on HN is far more damaging than owning the AI-assisted workflow.
See the clustr plan Section 5 for the full framing. It applies identically here.

The adaptr-specific version: "The HTML parser, framework detector, JS rewriter, and
gzip pipeline are structured Go code. The CI is green. The Docker quick-start works
on commodity hardware in 60 seconds. Run it against your SPA and tell me if it works.
That is the relevant test."

---

## 6. Failure Modes

### HN shadowban / post gains no traction

Same mitigation as clustr and tunnl: verify post visibility in an incognito window
within 5 minutes of posting. If shadowbanned, email hn@ycombinator.com. The posting
account must have established HN karma before launch day — see Phase B week 8 for
the founder engagement plan.

Backup if HN fails entirely: post to r/webdev with a "going to post this to HN but
something went wrong — here is the project" framing. Not ideal, but the r/webdev
community is large and the problem is directly relevant.

### A critical bug surfaces in the first hour

Same acknowledge-fast / fix-fast / thank-the-finder pattern as clustr and tunnl.
Dinesh ships a hotfix, Founder acknowledges in the thread, Dinesh updates the Docker
image tag. The bug is not the story. The response is.

The adaptr-specific P0 scenario: the Docker quick-start fails entirely for a common
SPA framework (React/Vite is the most likely). If `docker run -e TARGET=... -e
BASE_PATH=... ghcr.io/sqoia-dev/adaptr:latest` does not work against a vanilla CRA
or Vite app, that is a P0. Dinesh must have a fix in under 2 hours.

### Zero engagement (mediocre launch)

If the post gets under 20 upvotes and falls off the front page by hour 2:

1. Do not repost immediately.
2. Wait 6–8 weeks. Ship Sprint R with a meaningful new capability (full Next.js
   support, multi-SPA mode, or Brotli output — whichever is most visible).
3. The most likely failure mode specific to adaptr: the problem framing did not
   land. "Serve SPA at subpath" is abstract if the reader has not hit the problem.
   Reframe the title for the retry around a concrete error the reader has seen:
   "Show HN: adaptr – stop getting 404s when you serve a React app at /myapp/"
   or "Show HN: adaptr – the tool that fixes 'cannot GET /myapp/' without a rebuild."
4. Consider leading the re-launch with the technical blog post. Let it generate
   organic discussion first, then follow with a revised Show HN once the dev.to
   article and HN link post have established baseline credibility.

Erlich produces a two-paragraph debrief within 48 hours of a zero-traction launch.

---

## 7. Success Criteria

### "Good" outcome

- Post reaches the front page (top 30 on news.ycombinator.com) within the first
  4 hours.
- 100+ upvotes by end of launch day.
- 50+ comments, majority substantive and technical.
- 150+ GitHub stars in the first 24 hours.
- No P0 bugs surfaced that are not fixed within 2 hours.
- 3+ external people signal genuine interest in deploying it (GitHub issues with
  deployment questions, DMs, emails).
- At least one comment from someone who has personally hit the SPA subpath problem
  ("I spent two days on this last month — this is exactly what I needed").

### "Great" outcome

- Post stays on the front page for 6+ hours.
- 300+ upvotes.
- 600+ GitHub stars in the first week.
- A recognizable name in the HN / Go / frontend tooling community comments
  positively.
- An unsolicited blog post, tweet, or awesome-list submission from someone outside
  the team within 48 hours.
- GitHub star velocity sustains at 30+ per day through the end of the first week.
- A Kubernetes or OOD community member reports a successful production deployment
  within 72 hours.
- Helm chart install count reaches 50+ in the first week (K8s audience confirmed).

### "We under-shipped — retry in 6 months"

- Post gets under 20 upvotes and no front-page traction.
- Zero GitHub stars from people outside the team.
- No substantive comments or only comparison dismissals ("just use Vite base").

If this outcome occurs:

1. Do not repost the same framing. The problem statement or the title was wrong.
2. The most likely failure mode: the audience did not recognize the problem. The
   retry must lead with a concrete error message, not an abstract capability
   description. See Failure Modes section above.
3. Ship Sprint R. Full Next.js support or multi-SPA mode is the capability that
   changes the conversation.
4. Revisit whether the primary channel is HN or a more targeted community first
   (Reactiflux, K8s Slack, OOD discourse). HN is high-variance for infrastructure
   tooling with a specific use case.

---

## Compare-and-Contrast with clustr and tunnl Launches

### What makes the adaptr HN audience different

clustr targeted a small, expert HPC audience. The people who engaged knew the problem
immediately and had high institutional switching costs. A small engaged thread was
a good outcome.

tunnl targeted a mainstream developer tools audience who already had a solution
(ngrok). The challenge was differentiation, not problem recognition. A larger,
more skeptical audience with a known comparison point.

adaptr targets the broadest audience of the three: every SPA developer who has
deployed behind a reverse proxy. The problem (broken at `/myapp/`) is more
universally experienced than HPC provisioning gaps or webhook history. The challenge
is different from both:

- Unlike clustr: the problem IS widely recognized ("404 on subpath" is a StackOverflow
  search term with thousands of results). Problem recognition is not the barrier.
  The barrier is "I didn't know there was a container-level solution."
- Unlike tunnl: there is no single "household name" incumbent to position against.
  nginx sub_filter is not ngrok. It is a config option in a larger tool, not a
  competing product. The comparison is more nuanced and less emotionally charged.
- Unlike both: adaptr's quick-start is genuinely 60 seconds. The Docker one-liner
  is the fastest evaluation path of the three products. A sysadmin who has been
  fighting this problem can have adaptr running before they finish reading the HN
  thread. That is the single biggest conversion advantage.

The cross-linking advantage compounds here. By the time adaptr launches, tunnl and
clustr have posted. The founder has two HN thread performances behind them. The
"from the team that built clustr and tunnl" framing is social proof that no single-
product launch can have. Use it — in the opening comment, in the README, and in
warm outreach.

### Lessons to apply from the prior two launches

Before the adaptr Phase C warm outreach begins (week 21), Erlich must read both the
clustr and tunnl HN threads in full. Apply the clustr plan's week-13 observation
framework: which objections dominated the early thread? Which opening comment
structures got the most positive engagement? What was the star-to-GHCR-pull ratio
in the first 24 hours? The adaptr launch should benefit from two full datasets.

---

## Appendix: The One Thing About the adaptr HN Audience You Cannot Forget

**The adaptr HN reader has probably already solved this problem badly.**

They have a `sed -i` script in their CI pipeline. They have a `basename` prop
hardcoded in a React Router config file. They have an nginx location block with
`sub_filter` that works for most things except the WebSocket endpoint. They have
told the frontend team to "just set the Vite base" and the frontend team said the
path changes per session. They have written a Stack Overflow answer about this that
has 47 upvotes and still gets comments every month.

Their bar for adopting adaptr is not "does this solve the problem?" They know it
solves the problem. Their bar is: "Is this faster than the hack I already have?"

A 60-second Docker one-liner is the only correct answer to that question. If the
quick-start takes 10 minutes, or requires reading a 400-word doc first, or fails
on the first try, the reader goes back to their `sed -i` script because it works
and they already understand it.

The SHOW-HN-DRAFT.md quick-start section must be verified to work verbatim — on a
real SPA, on clean hardware, in under 60 seconds — before the post goes live. That
is the final gate. Everything else in this plan is preparation for that moment.
