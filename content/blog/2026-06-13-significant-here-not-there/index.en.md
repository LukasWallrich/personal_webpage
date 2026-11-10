---
title: "Significant here, not there: re-testing a classic diversity-and-teams finding"
author: Lukas Wallrich
date: '2026-06-13'
slug: significant-here-not-there
categories: [metascience]
tags: [Metascience, Replication]
summary: "Re-testing Timmerman's (2000) age-diversity x interdependence result with a direct interaction test: the sport difference survives, but the original significant-vs-not-significant inference was a fallacy."
authors: []
featured: no
math: true
image:
  caption: 'Interdependence vs. independence: linked basketball teammates, separate baseball players.'
  focal_point: 'Center'
  preview_only: no
projects: []
---

> **About this series — _AI-assisted replications_.** I care about two things that do not meet as often as they should: the credibility of published findings, and the effective use of agentic AI in research. In the course of other projects I repeatedly find myself replicating or re-analysing existing work, and in this series I write those efforts up. Each post is heavily co-drafted by Claude and then fully checked by me, and the checking is not a formality, as this first instalment happens to show. The code and data for every replication are openly available on the [OSF project](https://osf.io/3jz8k/). This is the first post.

Some claims in management research get repeated so often that they stop feeling like claims and start feeling like furniture. One of them is that diversity is more costly for teams whose members depend on each other: spread a team out in age, tenure, or background, and the highly interdependent teams, the ones that have to coordinate in real time, pay a price that more loosely coupled teams do not.

A much-cited early source for that idea (more than 250 citations on OpenAlex) is Timmerman {{< cite "-Timmerman2000" >}}, who used professional sport as a natural laboratory. Basketball players coordinate continuously, whereas baseball players mostly act in sequence, so the two settings differ sharply in task interdependence. Timmerman reported that age diversity (measured as a coefficient of variation, CV) was associated with lower win percentage in the NBA but not in MLB, and concluded that interdependence moderates the effect of age diversity. It is a clean and intuitive result, and it has been cited that way ever since.

There is a well-known statistical trap in that reasoning, however. Timmerman did not test the moderation directly. He fitted two separate hierarchical regressions, one per sport (team-ability controls first, then the diversity terms), found the age-diversity coefficient significant in basketball but not in baseball, and read the moderation off that contrast. As Gelman and Stern {{< cite "-GelmanStern2006" >}} put it, the difference between "significant" and "not significant" is not itself statistically significant: two estimates can fall on opposite sides of p = .05 while being statistically indistinguishable from one another.

So we did what the original analysis did not, and tested the interaction directly.

## Rebuilding the study

We reconstructed both samples on Timmerman's own 1950–1997 window, following his design exactly: the roster is the set of players appearing in more games than their team-season average (computed separately for pitchers and non-pitchers in baseball); age diversity is the coefficient of variation of player age; and the controls are team ability (on-base, slugging and earned-run averages for baseball, field-goal %, free-throw % and rebounding for basketball), plus mean age. From the `Lahman` database this gives **1,082 MLB team-seasons, the exact number Timmerman analysed**, with descriptives in the same ballpark as his (mean age 28.3 vs his 28.85; CV .137 vs .14). The reconstruction is close but not identical: we approximate player age from birth month rather than exact birthday at season's end, and for pre-1970 seasons we set missing hit-by-pitch and sacrifice-fly counts to zero when computing on-base percentage. For the NBA we paired public player ages with FiveThirtyEight's Elo win percentages, giving 826 team-seasons (against his 871). Using a common window matters: different time spans would confound the sport contrast with era. The within-sport coefficients reproduce his pattern — age diversity is unrelated to winning in baseball (β = −.0010, p = .30) and negatively related in basketball (β = −.0181, p < .001). (These are semi-standardised slopes: the diversity measure is standardised but win percentage is left on its native scale.)

This is where we have to be honest about our own first pass. The naive pooled interaction, the one we wrote down without thinking carefully, came out resoundingly null (β = −.002, p = .71). For a moment that looked like a tidy debunking: the celebrated moderation was apparently a difference-of-significance mirage.

It was not. That p = .71 turned out to be an artefact of our own model. Mean age predicts winning far more strongly in basketball than in baseball, and our first pooled model forced a single common slope for mean age across both sports. Because age spread and mean age travel together (older rosters are also more varied in age), that misspecification bled into, and cancelled, the very interaction we were testing. Once the mean-age relationship is allowed to differ by sport, as it plainly should, the age-diversity by interdependence interaction returns to significance (p = .007, or p = .010 with heteroskedasticity-robust standard errors). The cleanest version of the test, the difference between the two sports' standardised slopes (each estimated with its own ability controls), agrees: β_NBA − β_MLB = −.0171, 95% CI [−.027, −.007], p < .001. Timmerman's *direction* holds up.

So is the stylised fact vindicated? Only in a qualified sense, and two caveats matter more than the headline. The first is effect size: age diversity adds about 1.3 percentage points to the explained variance in NBA win percentage (ΔR² = .013) — a real but slight contribution. The *sign* repays a closer look, because it runs against the intuitive confound story. The raw correlation between age spread and winning is in fact slightly positive (+.05): older rosters tend to be both more age-diverse (r = .32) and considerably more successful (r = .42), and that shared dependence on experience props the simple correlation up. Only once mean age is held constant does a small negative association emerge (β = −.014 controlling for mean age alone; −.0181 with the full set of ability controls). So adjusting for age here does not manufacture the effect; it strips out a positive confound that was masking it. This is still an adjusted observational association, not a causal estimate, but the negative partial association holds up under the controls Timmerman used.

The second caveat is deeper, and Timmerman flagged it himself: sport is perfectly confounded with interdependence. Basketball and baseball differ in countless ways besides how tightly players must coordinate (roster size, scoring structure, career length, what "ability" even means), so a difference in age-diversity slopes between them is *consistent with* the interdependence account but cannot isolate it. What the data show is a genuine sport difference in the age-diversity effect; reading that difference as interdependence is an interpretation the design cannot adjudicate.

Two conclusions follow. First, the original inference was a fallacy: Timmerman reached a conclusion that happens to survive, but his analysis could not have told him so, and that is luck rather than method. Second, a debunking is as fragile as the claim it targets. Our own "it's null" nearly became a confident finding until a single misspecified covariate gave itself away. Both the effect and the easy refutation were one modelling decision deep.

One caveat throughout: for baseball the reconstruction is close (the same 1,082 team-seasons, the same design and controls), so the null there is a faithful reproduction, with the minor approximations noted above (birth-month ages, pre-1970 on-base percentages). The NBA half is further from the original: the same 1950–1997 window, but win percentages from FiveThirtyEight's Elo game logs rather than his records, 826 teams against his 871, and a rebounding control built from season-total rebounds rather than his per-game figure. We also did not rebuild the racial-diversity half of his paper. We are testing whether the phenomenon reproduces, not auditing his spreadsheet; on that test the sport difference is real, fragile, and small, and it is *consistent with* the interdependence story Timmerman told, which, given that the design cannot separate interdependence from everything else that distinguishes the two sports, is about the most one can ask of a stylised fact.

---

## Methods note: can confidence intervals around the two slopes test the interaction?

A natural question is whether we can judge the moderation from confidence intervals on the two per-sport slopes, rather than from a pooled interaction term.

**The pooled interaction is the test.** Moderation is the claim that the age-diversity slope *differs* between the low- and high-interdependence sports. An interaction term in a single model pooling both sports estimates exactly that difference, with its own standard error and p-value. Nothing else is needed, provided the model is specified correctly. The lesson we learned the hard way is that every covariate whose effect plausibly differs across the moderator (here, mean age) must also be allowed to interact; otherwise the constrained covariate contaminates the interaction of interest.

**The interval route works, but not the intuitive version of it.** The tempting check is whether the two per-sport confidence intervals overlap. That test is wrong, and conservatively so: two estimates can have comfortably overlapping 95% intervals and still differ significantly {{< cite "SchenkerGentleman2001" >}}. Overlap is not the question. The correct interval-based test is a confidence interval on the *difference* of the two independent slopes. For independent estimates,

  β_NBA − β_MLB,  SE_diff = √(SE²_NBA + SE²_MLB),

and the test is whether that difference excludes zero. This is algebraically the pooled interaction (up to the choice of pooled versus separate error variance), so the two should agree.

We computed it (standardised CV slopes, each sport with its own controls):

- MLB: β = −.0010, SE = .0009
- NBA: β = −.0181, SE = .0049
- Difference (NBA − MLB): β = −.0171, SE = .0050, z = −3.44, p < .001, 95% CI [−.027, −.007]

The interval excludes zero, so the slopes differ and the moderation is supported. The individual intervals (MLB [−.0028, +.0008], NBA [−.0277, −.0085]) happen not to overlap here, but that is not what we are relying on; the interval on the difference is the test. Using each sport's own error variance matters, because NBA win percentage is roughly twice as variable as MLB's (SD .15 vs .07); the homoskedastic pooled model that assumes one common variance, together with the common mean-age slope, is precisely what produced the spurious p = .71. The robust pooled interaction (p = .010) and the difference-of-slopes test (p < .001) agree in rejecting the null, and both are preferable to the naive common-slope model. (The two p-values differ in size because the difference-of-slopes test gives each sport its full set of ability controls, whereas the pooled-interaction illustration uses mean age as the only shared covariate; both clearly support the moderation.)

*Code and data are openly available on the [OSF project](https://osf.io/3jz8k/), which rebuilds both datasets from public sources and reproduces every number above.*

### References
{{< bibliography >}}
