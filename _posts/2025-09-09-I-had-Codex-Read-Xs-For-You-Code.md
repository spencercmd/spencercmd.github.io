---
layout: default
title: "I had OpenAI's Codex CLI read X's Newly Open Sourced 'For You' Codebase. Here is what it found to help you drive engagement."
date: 2025-09-09 15:00:00 -0000
---

# {{ page.title }}

**Published:** {{ page.date | date: "%B %d, %Y" }}

---

 # How “For You” Works

  - Candidate sourcing: Pulls tweets from multiple sources (in‑network, recs via FRS/UTEG, communities, popular videos,
  backfills).
  - Filtering: Removes/filters spam, unsafe, low‑quality, duplicates, location/visibility misfits, and fatigue.
  - Scoring: ML models predict engagement probabilities (per “head”), then a weighted reranker combines them into a
  final score.
  - Reranking: Scores are combined with per‑head normalization, biases/debiasing, and special handling of negative
  feedback.
  - Assembly: Ads and modules inserted; client decorations applied.

  # Key Ranking Signals (explicit in code)

  - Positive heads (optimize for these):
      - Favorites, replies, retweets.
      - Good clicks (quality click signals), profile clicks.
      - Shares, bookmarks.
      - Dwell on tweet detail.
      - Video: quality views (≥10s when “ten seconds logic” enabled), immersive video views, watch time.
  - Negative head (avoid these):
      - Predicted Negative Feedback V2 (mutes, blocks, “show less”, etc.).
  - Feature gates/thresholds:
      - Video quality view eligibility often requires video ≥10 seconds.
      - Dwell and VQV can be thresholded or binarized; low dwell can be reduced to zero contribution.
  - Weights and normalization:
      - Weights come from feature switches (set server side); model heads are per‑head max‑normalized then linearly
  combined.
      - Negative weights are filtered by thresholds (constant or normalized) and can still subtract when above
  threshold.
  - Contextual features commonly used:
      - RealGraph/in‑network scores, social proof (liked/retweeted/replied‑by counts + user IDs), author features
  (verification/creator), media type, embeddings (SimClusters), language match, topic and “Grok” content understanding
  flags (NSFW, spam, low quality, gore/violence).

  # Downranks and Drops (visibilitylib)

  - Strong enforcement against: spam, low quality, engagement spamming, copypasta, unsafe/NSFW, crisis/misinformation
  policies.
  - Downrank reasons include spam replies, low‑quality mentions, high spammy content scores, and other health signals.
  - Feedback fatigue scorer reduces scores for entities the viewer previously “See fewer” on (authors, likers/followers/
  retweeters).

  # High‑Leverage Tactics

  - Drive “high‑value” engagements
      - Bookmarkable content: deep guides, tools, cheat sheets, templates; explicitly invite saving for later.
      - Shareable posts: concise value + strong hook; include context that travels well.
      - Profile clicks: craft a compelling bio/pinned post; occasionally ask readers to check your profile for related
  threads/resources.
      - “Good clicks” and dwell: use clear previews that reward opening tweet details (not clickbait); add structured
  info that takes time to absorb.
  - Win video signals
      - Post videos ≥10 seconds to qualify for “video quality viewed”; optimize early seconds to reduce drop‑off.
      - Tight edits, clear audio, subtitles; deliver a payoff before 10–15s to maximize quality watch and watch time.
      - Use immersive-friendly formats where relevant (portrait, clean framing).
  - Activate in‑network and social proof
      - Publish when your core audience is online to maximize fast, in‑network engagement.
      - Seed engagement via collaborators/mutuals; early likes/retweets/replies from your network improve candidate
  viability.
      - Nurture follower quality: interact meaningfully so RealGraph/relationship strength grows.
  - Keep language and topic alignment tight
      - Match audience language; avoid mismatched multilingual posts in the same tweet if your audience is mostly one
  language.
      - Stay within coherent topic clusters; recurring themes help embedding/topic features and candidate recall.
  - Avoid negative feedback at all costs
      - Don’t reply‑spam, keyword‑stuff, or post near‑duplicate/copypasta content.
      - Steer clear of misleading/low‑quality bait; health and policy rules can downrank or drop visibility.
      - Watch “See fewer” patterns; if a format triggers fatigue, rotate topic/format.
  - Format for consumption and dwell
      - Threads: lead with substance in tweet 1; maintain tight pacing; summarize at end with a save/share prompt.
      - Use clean visuals and readable text over images; avoid heavy image text that seems spammy/low‑quality.
  - Timing and cadence
      - Favor windows when your core audience engages; consistency helps both candidate recall and social proof.
      - Don’t flood: fatigue filters and deduplication reduce returns from repetition.

  # What Not To Do

  - Don’t farm weak clicks; “good clicks” and dwell are what matter, not empty opens.
  - Don’t rely on pure “engagement bait” (e.g., low‑effort polls, rote prompts); it risks low quality labels or negative
  feedback.
  - Don’t mass‑reply for exposure; spam‑reply downranks are explicit.
  - Don’t post borderline NSFW/violent/gore content; related labels suppress For You ranking.
  - Don’t re‑post the same asset repeatedly; dedup + fatigue scorers will discount it.

  # Quick Audit Checklist

  - Does each post deliver one of: bookmark value, share value, or a profile‑worthy call‑to‑learn‑more?
  - For video: ≥10s, strong first 3–5s, captions, clear payoff.
  - Thread structure: front‑loaded value, skimmable formatting, explicit takeaways.
  - Language/topic: aligned with audience; consistent themes.
  - Early engagement plan: alert key mutuals; schedule at high‑engagement times.
  - Health: zero spammy patterns; compliant with safety policies.

  # Measure And Iterate

  - Track rate per impression for: bookmarks, shares, profile clicks, dwell, video quality views/watch time.
  - Compare formats: single vs thread; short vs long video; CTAs (save/share/profile).
  - Watch audience‑fit signals: who engages early, language distribution, topic resonance.
  - Kill formats that produce “See fewer” or high bounce/low dwell.



  -----------
   
   # Candidate Sources

  - TweetMixer (ScoredTweetsTweetMixer): General Home recs with reply/directed‑at filters and retweet dedup. Good for
  broad recall once there’s early engagement.
  - UTEG (ScoredTweetsUteg): User‑Tweet Engagement Graph (liked/retweeted by socially close accounts). Strong social
  proof helps; “reply targeting” can be filtered out (see filters).
  - Earlybird In‑Network (ScoredTweetsEarlybirdInNetwork): Recent tweets from followed users; a key starter for early,
  in‑network engagement.
  - Popular Videos (ScoredTweetsPopularVideos): Explore Ranker immersive video recs; favors quality watch/immersion.
  - Lists/Communities/Backfill/Cached: Niche or fallback sources that round out supply.

  # High‑Impact Filters (what gets removed or discounted)

  - Replies/directed‑at misfit
      - QualifiedRepliesFilter: Removes replies by followed authors when the reply targets a user the viewer does not
  follow and the reply is a retweet, a reply‑to‑reply/retweet, or not a valid reply target. Translation: reply‑chains
  that look low‑quality or misdirected get filtered.
      - ExtendedDirectedAtFilter: Removes tweets “directed at” someone the viewer doesn’t follow when author is followed
  by the viewer. Translation: @-shots at non‑followed users from people you follow are reduced.
  - Deduplication and hydration
      - RetweetDeduplicationFilter: Multiple RTs of the same original are deduped; original or the highest‑scored
  retweet wins. Translation: posting original content beats chasing retweet variants.
      - TweetHydrationFilter: Drops tweets that fail Tweetypie hydration. Translation: broken media/metadata kills
  eligibility.
  - Location and fatigue
      - LocationFilter: If your tweet has a location and the user has a location, they must match. Translation: avoid
  unnecessary/incorrect location tags that shrink reach.
      - FeedbackFatigueFilter and FeedbackFatigueScorer: Viewers’ “See fewer” on your author, likers, followers, or
  retweeters causes filtering or score discounting for a decay window.
  - Content quality/health
      - GrokNsfwFilter, GrokViolentFilter, GrokGoreFilter, GrokSpamFilter: Content understanding labels can drop/
  downrank candidates.
  - Media hygiene
      - MinVideoDurationFilter/MaxVideoDurationFilter: For video quality signals (e.g.,
  PredictedVideoQualityWatchScoreFeature), ≥10s duration often required when the “ten seconds logic” is enabled.
  - Previously‑seen/similar
      - PreviouslySeen* and cluster/media dedup filters: Reduce repeats, near‑dupes, and media repetition.

  # Scoring Heads To Optimize (combined in the reranker)

  - Positive model heads (maximize via content and structure):
      - Favorites, replies, retweets, “good clicks” (quality click variants), profile clicks, shares, bookmarks, dwell
  on tweet detail, video quality views (≥10s), immersive video views, watch time.
  - Negative head (avoid):
      - PredictedNegativeFeedbackV2ScoreFeature (mutes/blocks/show‑less).
  - Reranking logic:
      - Heads weighted via Feature Switch params (ModelWeights.*), per‑head max normalization, and optional bias/debias
  tensors; negative weights are gated by normalized/constant thresholds and can still subtract when above threshold.

  # The “For You Readiness Score” (FYRS)

  Use this quick pre‑post checklist to approximate the ranking heads with controllable proxies. Aim for FYRS ≥ 8 to
  post; do not post if any “Blockers” trigger.

  - Value (0–6)
      - +3 Bookmark value: concrete utility that merits saving (guide, template, code, checklist).
      - +2 Share value: concise, portable insight that stands alone if shared.
      - +1 Profile pull: clear reason to view your profile (related thread/pinned resource).
  - Dwell/Click Quality (0–5)
      - +2 Strong first line/hook that previews substance (not clickbait).
      - +2 Skimmable structure (short paras, bullets, or thread with summaries).
      - +1 Visual clarity (clean image/video captions, no dense text‑in‑image).
  - Social Proof Setup (0–4)
      - +2 Publish in a window where 5–10 core followers are active.
      - +1 Relevance to your recurring topics (consistent cluster/theme).
      - +1 One lightweight nudge to genuine peers (no mass tags/asks).
  - Video Only (0–4; if applicable)
      - +2 Duration ≥10s with clear payoff by 5–10s.
      - +1 Captions/subtitles; +1 portrait/immersive‑friendly framing.
  - Safety/Health and Fatigue (−8 to 0)
      - −3 Any risk of “See fewer” (you posted same topic ≥3x this week with weak engagement).
      - −3 Looks like reply/directed‑at bait (reply to non‑followed or @ aimed at non‑followed without context).
      - −2 Near‑duplicate to recent post or recycled media.
  - Integrity Blockers (hard stop)
      - NSFW/violent/gore/spammy flags likely; misleading claims; broken media; unnecessary location tag; fails
  hydration in preview. If any true, do not post.

  Target: FYRS ≥ 8 and no Blockers.

  # How To Score Yourself (fast pass)

  - If thread: lead tweet delivers standalone value; later tweets deepen value; end with summary + “save/share”.
  - If single image: conveys an idea that invites a save or a share; caption adds context.
  - If video: ≥10s, substantive takeaway by 5–10s, captions on, cover frame communicates the value.

  # Source‑Fit Micro‑Checks

  - In‑network kickstart (Earlybird/In‑Network): Will at least 5 core followers likely engage in 15–30 minutes?
  - UTEG/social proof: Is the topic aligned to people who recently liked/retweeted you? Avoid low‑quality reply chains
  or @‑shots to non‑followed accounts (tripped by QualifiedRepliesFilter/ExtendedDirectedAtFilter).
  - Popular video lane: If video, does it pass the 10s quality view bar and feel immersive?

  # Operational Guardrails

  - Post cadence: Avoid posting the same asset/angle repeatedly (fatigue + dedup filters).
  - Location: Don’t attach location unless it’s critical and broadly relevant to your audience.
  - Retweets vs original: Prefer original content; retweets of the same origin are deduped.
  - Language/topic: Keep consistent language and coherent topics; avoid mixed‑language posts that don’t match audience.

  # How To Make It Testable

  - Track per‑post “high‑value” rates you can infer:
      - Bookmarks, shares, profile clicks, dwell proxy (detail expands), video 10s+ views/watch time.
  - After 5–10 posts, calibrate FYRS thresholds:
      - If posts with FYRS ≥ 9 outperform median impressions per follower and show higher bookmark/share rates, your
  rubric is aligned.
      - Tighten negatives if “See fewer” or muted patterns rise.