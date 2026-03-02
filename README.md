# Analysis 2: Supporting Measurement Analysis (Candidate vs. Keyword) + Preliminary Harm Proxy

This section extends the project by adding (1) a supporting measurement analysis comparing candidate-based and keyword-based collection during the election-week surge window, and (2) a preliminary Reddit-based harm proxy to explore whether surge periods correspond with changes in harm-flagged language rates. These analyses help interpret the main cross-platform surge findings by showing how measurement choices and text-based signals can shape the meaning of "election-related activity."

The first part of this section is a supporting analysis rather than a standalone research question: it helps contextualize RQ1 and RQ2 by comparing how different collection strategies represent activity in the same time window. The second part addresses RQ3 (exploratory) by testing whether a lightweight harm indicator varies during the shared surge period identified in Analysis 1.

# Data Assembly (Analysis 2)
This section uses two related data inputs. For the candidate-vs-keyword comparison, I combine (a) daily keyword counts derived from the tidy index (df_daily) and (b) candidate daily counts assembled from MEIU22 candidate files for Facebook and Twitter. These are merged into a common daily structure (platform × day × collection_type × count) so that volumes can be compared within the same election-week window.

For the preliminary harm proxy (RQ3, exploratory), I use Reddit text records from the assembled workflow where day-level coverage is available. This allows me to compute a simple daily harm-flag rate and compare it to the shared surge days identified in Analysis 1. The Reddit harm proxy is intentionally lightweight and is used here as an exploratory signal, not a validated toxicity or hate-speech classifier.

| source_glob | files_found | sample_files (first 3) |
|---|---:|---|
| `/content/MEIU22/data/facebook_candidate/*.csv.gz` | 208 | `/content/MEIU22/data/facebook_candidate/post_id_by_candidates_2022-06-01.csv.gz`<br>`/content/MEIU22/data/facebook_candidate/post_id_by_candidates_2022-06-02.csv.gz`<br>`/content/MEIU22/data/facebook_candidate/post_id_by_candidates_2022-06-03.csv.gz` |
| `/content/MEIU22/data/twitter_candidate/*.csv.gz` | 208 | `/content/MEIU22/data/twitter_candidate/tweet_id_by_candidates_2022-06-01.csv.gz`<br>`/content/MEIU22/data/twitter_candidate/tweet_id_by_candidates_2022-06-02.csv.gz`<br>`/content/MEIU22/data/twitter_candidate/tweet_id_by_candidates_2022-06-03.csv.gz` |

| index | platform | day | collection_type | count |
|---:|---|---|---|---:|
| 0 | facebook | 2022-06-01 | candidate | 1336 |
| 1 | facebook | 2022-06-02 | candidate | 1303 |
| 2 | facebook | 2022-06-03 | candidate | 1339 |
| 3 | facebook | 2022-06-04 | candidate | 903 |
| 4 | facebook | 2022-06-05 | candidate | 729 |

| index | platform | day | count | collection_type |
|---:|---|---|---:|---|
| 0 | facebook | 2022-10-01 | 1440 | keyword |
| 1 | facebook | 2022-10-02 | 1365 | keyword |
| 2 | facebook | 2022-10-03 | 2400 | keyword |
| 3 | facebook | 2022-10-04 | 2783 | keyword |
| 4 | facebook | 2022-10-05 | 2912 | keyword |


# Understanding the Inputs (Comparability and Coverage for Analysis 2)
Before comparing collection strategies, I reviewed which platform/day combinations are comparable within the election-week window (Nov 4–10). The candidate's daily files provide date-embedded counts for Facebook and Twitter, while keyword daily counts come from the tidy index. This allows you to compare Facebook candidate vs. keyword volumes directly in the same window.

A key limitation remains: the Twitter keyword sample in the tidy index lacks usable day-level timestamps, so Twitter appears only in the candidate-vs-keyword window comparison, along with candidate totals. Rather than forcing an invalid comparison, I treat this as a measurement constraint and explicitly note it in both the table/figure interpretation and the project limitations.

# Analysis 2A: Supporting Measurement Analysis (Candidate vs. Keyword)
This analysis compares candidate-based and keyword-based collection during the election-week surge window (Nov 4–10) to show how the collection strategy changes the apparent scale of election-related activity. The goal is not to replace the main surge analysis, but to provide a measurement-oriented interpretation layer that explains why counts can differ substantially depending on how the data are assembled. To do this, I merge candidate daily counts and keyword daily counts into a pivoted table by platform and day, then compute a difference column (candidate - keyword) where both streams are available. I also compute total counts by platform and collection type across the Nov 4–10 window to summarize the magnitude of the measurement difference and to support a simple comparison figure.

| index | platform | day | candidate | keyword | diff_candidate_minus_keyword |
|---:|---|---|---:|---:|---:|
| 0 | facebook | 2022-11-04 | 2182.0 | 9235.0 | -7053.0 |
| 1 | facebook | 2022-11-05 | 1948.0 | 7373.0 | -5425.0 |
| 2 | facebook | 2022-11-06 | 1729.0 | 6738.0 | -5009.0 |
| 3 | facebook | 2022-11-07 | 2370.0 | 15811.0 | -13441.0 |
| 4 | facebook | 2022-11-08 | 3527.0 | 27073.0 | -23546.0 |
| 5 | facebook | 2022-11-09 | 1318.0 | 23499.0 | -22181.0 |
| 6 | facebook | 2022-11-10 | 805.0 | 10378.0 | -9573.0 |
| 7 | twitter | 2022-11-04 | 5604.0 | NaN | NaN |
| 8 | twitter | 2022-11-05 | 4366.0 | NaN | NaN |
| 9 | twitter | 2022-11-06 | 4022.0 | NaN | NaN |

| index | platform | collection_type | count |
|---:|---|---|---:|
| 0 | facebook | candidate | 13879 |
| 1 | facebook | keyword | 100107 |
| 2 | twitter | candidate | 34886 |



<img width="989" height="590" alt="image" src="https://github.com/user-attachments/assets/a7a4849b-0921-487a-9ae5-b29e59cdc6c2" />

# Interpreting the Results (Supporting Measurement Analysis)
The candidate-vs-keyword comparison shows that the collection strategy can materially change the apparent magnitude of election-related activity in the same time window. In the Nov 4–10 surge window, the Facebook keyword stream (100,107 items) is substantially larger than the Facebook candidate stream (13,879 items), suggesting that keyword collection captures a much broader layer of election-related activity than candidate-post-only collection.

This result is important for interpreting the project's main findings because it reinforces the idea that cross-platform comparisons are, in part, measurement comparisons. Differences in observed activity may reflect both platform dynamics and differences in what each collection strategy includes or excludes. For that reason, I treat this as a supporting measurement analysis that helps contextualize RQ1 and RQ2 rather than as a separate formal research question.

Twitter appears in the window totals with candidate counts only (34,886) because the Twitter keyword sample in the tidy index lacks usable day-level timestamps. This limitation prevents a clean, daily comparison of Twitter candidates vs. keywords in the current workflow and is carried forward into the project's limitations and next steps.

# Analysis 2B: Preliminary Harm Proxy on Reddit (RQ3, Exploratory)
To address RQ3 (exploratory), I use a lightweight text-based harm proxy on Reddit content to examine whether days with strong election-related surges correspond with changes in rates of harm-flagged language. Reddit is used here because text is available in a form that can be analyzed directly within the current workflow, and Reddit also had valid day-level coverage in Analysis 1.

The proxy is computed by creating a text field for screening, flagging comments that match a predefined set of harm-related terms/patterns, and then aggregating daily totals into three measures: total_comments, harm_comments, and harm_rate (harm-flagged comments divided by total comments). I then compare these daily values across the full available period and focus on the election-week surge window and shared surge dates from Analysis 1.

| index | day | total_comments | harm_comments | harm_rate |
|---:|---|---:|---:|---:|
| 0 | 2022-10-01 | 54 | 4 | 0.074074 |
| 1 | 2022-10-02 | 49 | 3 | 0.061224 |
| 2 | 2022-10-03 | 79 | 5 | 0.063291 |
| 3 | 2022-10-04 | 80 | 9 | 0.112500 |
| 4 | 2022-10-05 | 87 | 1 | 0.011494 |
| 5 | 2022-10-06 | 85 | 3 | 0.035294 |
| 6 | 2022-10-07 | 75 | 3 | 0.040000 |
| 7 | 2022-10-08 | 82 | 3 | 0.036585 |
| 8 | 2022-10-09 | 57 | 3 | 0.052632 |
| 9 | 2022-10-10 | 92 | 5 | 0.054348 |

| index | day | total_comments | harm_comments | harm_rate |
|---:|---|---:|---:|---:|
| 76 | 2022-12-16 | 282 | 28 | 0.099291 |
| 77 | 2022-12-17 | 280 | 23 | 0.082143 |
| 78 | 2022-12-18 | 300 | 32 | 0.106667 |
| 79 | 2022-12-19 | 304 | 25 | 0.082237 |
| 80 | 2022-12-20 | 324 | 27 | 0.083333 |
| 81 | 2022-12-21 | 294 | 27 | 0.091837 |
| 82 | 2022-12-22 | 355 | 29 | 0.081690 |
| 83 | 2022-12-23 | 299 | 23 | 0.076923 |
| 84 | 2022-12-24 | 292 | 28 | 0.095890 |
| 85 | 2022-12-25 | 275 | 18 | 0.065455 |

| index | day | total_comments | harm_comments | harm_rate |
|---:|---|---:|---:|---:|
| 34 | 2022-11-04 | 4026 | 290 | 0.072032 |
| 37 | 2022-11-07 | 6551 | 354 | 0.054038 |
| 38 | 2022-11-08 | 12258 | 545 | 0.044461 |
| 39 | 2022-11-09 | 19804 | 726 | 0.036659 |
| 40 | 2022-11-10 | 10712 | 474 | 0.044249 |

<img width="1189" height="590" alt="image" src="https://github.com/user-attachments/assets/18abcf13-294b-470f-95c1-6246020b048d" />

|index|day|total\_comments|harm\_comments|harm\_rate|
|---|---|---|---|---|
|34|2022-11-04 00:00:00|4026|290|0\.07203179334326876|
|35|2022-11-05 00:00:00|3186|219|0\.0687382297551789|
|36|2022-11-06 00:00:00|3409|232|0\.06805514813728367|
|37|2022-11-07 00:00:00|6551|354|0\.05403755151885208|
|38|2022-11-08 00:00:00|12258|545|0\.044460760319791154|
|39|2022-11-09 00:00:00|19804|726|0\.03665926075540295|
|40|2022-11-10 00:00:00|10712|474|0\.04424943988050784|

# Interpreting the Results (RQ3, Exploratory)
The preliminary Reddit harm proxy shows measurable day-to-day variation in harm-flagged language rates during the election-week surge period. In the Nov 4–10 window, Reddit comment volume rises sharply (especially on 11-08-2022 and 11-09-2022), while the harm rate declines across the peak volume days in the current proxy outputs (e.g., from 0.0720 on 11-04-2022 to 0.0367 on 11-09-2022, with a small rebound on 11-10-2022).

This pattern suggests that surge periods and harm-related signals may vary together, but not in a simple "more volume = higher harm rate" way. The relationship appears dynamic and potentially sensitive to the composition of the discussion during high-attention periods. Because the current indicator is a lightweight proxy, these results should be interpreted as exploratory evidence that motivates stronger follow-up measurement rather than as a basis for strong causal or diagnostic claims.

Even in this preliminary form, the RQ3 analysis is useful because it links the temporal surge findings from Analysis 1 to a potential escalation dimension. It functions as a proof-of-concept showing that the project can move beyond volume and timing to examine whether peak attention windows also correspond with measurable changes in language conditions.

# Figures and Tables Used in Analysis 2
This section uses one figure and two tables for the supporting measurement comparison, and one figure and one table for the preliminary Reddit harm proxy. Figure 3 and the candidate-vs-keyword summary tables support the measurement interpretation by showing how the collection strategy changes the apparent scale of activity during the Nov 4–10 surge window. Figure 4 and the Reddit harm proxy daily table support the exploratory RQ3 analysis by showing day-to-day variation in harm-flagged language rates across the study window and during shared surge days.

# Conclusion 
This section strengthens the project in two ways. First, the supporting candidate-vs-keyword comparison shows that measurement strategy materially changes what "election-related activity" looks like, which is essential context for interpreting the cross-platform volume and surge results from Analysis 1. Second, the preliminary Reddit harm proxy demonstrates that the workflow can begin to test whether surge periods correspond with shifts in harm-related language rates, even though the current proxy remains exploratory.

These analyses support the project's broader argument that cross-platform election attention should be interpreted through both platform dynamics and measurement design. The conclusion section in README synthesizes these findings and outlines how the project can be extended with stronger Twitter time coverage, formal lead-lag testing, and improved harm measurement.
