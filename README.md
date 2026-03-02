# Analysis 2: Supporting Measurement Analysis (Candidate vs. Keyword) + Preliminary Harm Proxy

This section extends the project by adding (1) a supporting measurement analysis comparing candidate-based and keyword-based collection during the election-week surge window, and (2) a preliminary Reddit-based harm proxy to explore whether surge periods correspond with changes in harm-flagged language rates. Together, these analyses help interpret the main cross-platform surge findings by showing how measurement choices and text-based signals can shape the meaning of "election-related activity."

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


