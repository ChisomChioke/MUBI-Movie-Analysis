# Genre Scaling Risk in Streaming Content: A Statistical Analysis of Production Volume vs Quality in MUBI Movies

## Table of Contents
- [Project Overview](#project-overview)
- [Business Problem](#business-problem)
- [Data Overview](#data-overview)
- [Analytical Approach](#analytical-approach)
- [Executive Summary of Results](#executive-summary-of-results)
    - [Key Findings](#key-findings)
    - [Edge Case Handling](#edge-case-handling)
- [Why This Project Matters](#why-this-project-matters)
- [Key Takeaways](#key-takeaways)
- [Limitations & Future Work](#limitations--future-work)
- [Technologies Used](#technologies-used)

## Project Overview

Streaming platforms face a critical question: **does scaling content production compromise quality?** Using **226,575 movies** across 19 genres spanning 1878-2021, I built a statistical framework analyzing 1990-2019 trends, revealing that **aggregate metrics mask quality degradation during rapid expansion**—Documentary's 3.37 overall rating hides an **8.4% decline** during 11x volume growth.

**Full Analysis Notebook:** [Link to Jupyter Notebook](https://github.com/ChisomChioke/MUBI-Movie-Analysis/blob/main/Movie_Data_Analysis.ipynb)

**One-Page Summary:** [View Project Summary](https://drive.google.com/file/d/14hrJJUFomB6r9yixJ32uZ06Z9TU_9A4M/view)

## Business Problem
Content platforms rely on aggregate ratings to guide expansion decisions, but **cross-sectional metrics mask temporal quality erosion.** Documentary appeared high-performing (3.37 rating) while actively declining from 3.57 to 3.27 during 11x expansion—potentially triggering misguided investment that accelerates brand dilution.

**Critical Questions:**
1. Does production volume systematically predict content quality?
2. Which genres can scale safely vs. those requiring strict curation?
3. How should platforms allocate resources across genres with different scaling dynamics?

Traditional cross-sectional analysis would recommend expanding Documentary based on its strong 3.37 aggregate rating. However, **85% of Documentary films were produced during 1990-2019,** when quality declined significantly (r = -0.937, p = 0.006). Aggregate metrics mislead strategy when they mask temporal trajectories.

## Data Overview

**Primary Dataset:** [MUBI SVOD Platform Database](https://www.kaggle.com/datasets/clementmsika/mubi-sqlite-database-for-movie-lovers)

**Supplementary Data:** [TMDB Movie Dataset](https://www.kaggle.com/datasets/asaniczka/tmdb-movies-dataset-2023-930k-movies) (genre enrichment)

- **Raw data:** 226,575 movies | 15.5M user ratings
- **After genre enrichment:** 139,237 movies with genre data (61% match rate)
- **Temporal scope:** 1990-2019 analysis (2020-2024 excluded due to 95% data discontinuity)
- **Genres analyzed:** 19 genres including Drama, Comedy, Documentary, Horror, Thriller

#### Key Variables:
- **Rating:** 1-5 scale user ratings (aggregate: mean 3.37)
- **Popularity:** Number of users who "love" the movie
- **Volume:** Movie count per genre per time period
- **Engagement:** Average likes per user critique

#### Data Preparation:
- Fuzzy matching merged MUBI ratings with TMDB genres (61% match rate)
- Multi-genre attribution: 52% of films assigned to multiple genres (weighted analysis)
- Temporal binning: 6 five-year periods (1990-2019) for trend analysis
- 2020-2024 excluded: 95% volume collapse indicates incomplete data

![Rated vs Unrated Movies](images/proportion_of_rated_vs_unrated_movies.png)
_Figure 1: 63% of movies have user ratings, while 37% remain unrated—indicating potential discoverability gaps and opportunities for targeted promotion._

## Methodology
#### Dual-Analysis Framework:

#### Phase 1: Cross-Sectional Benchmarking
- Correlation analysis (Pearson/Spearman) testing volume-quality relationships
- OLS regression: Genre rating ~ Production volume
- T-tests comparing high-volume vs. low-volume genres

#### Phase 2: Temporal Trend Detection
- Within-genre time-series analysis (1990-2019)
- OLS regression: Quality/Popularity ~ Time period
- Volatility analysis: Genre stability rankings
- Correlation: Production growth vs. quality change

#### Phase 3: Engagement Analysis
- Critique volume vs. engagement quality correlation
- High vs. low volume group comparisons
- Rating-engagement relationship testing

#### Statistical Methods:
- Pearson & Spearman correlation (relationship strength)
- OLS time-series regression (trend significance)
- Welch's t-tests (group comparisons)
- Volatility metrics (genre stability)

## Key Findings

**Finding #1: Cross-Sectional Analysis Misleads Strategy**

![Cross-Sectional Analysis](images/average_rating_vs_popularity_by_genre_cross_sectional.png)
_Figure 2: Cross-sectional view suggests Documentary (3.37 rating, 14,509 films) is a safe expansion target. However, this aggregate metric masks an 8.4% quality decline during 11x growth (1990-2019)._

**Cross-sectional correlation:** r = 0.175, p = 0.472 (not significant)
- No systematic volume-quality relationship detected
- High-rated genres span full volume spectrum
- Documentary (3.37, 14,509 films) vs. Music (3.17, 3,842 films)

**Critical limitation:** Aggregate ratings combine stable early period (3.57, 429 movies, 1990-1994) with degraded recent period (3.27, 4,836 movies, 2015-2019), masking temporal erosion.

**Strategic implication:** Relying on cross-sectional metrics alone would trigger Documentary expansion during active quality decline.

---

**Finding #2: Temporal Scaling Risks Are Genre-Specific**

![Documentary Trend](images/documentary_decline.png)
_Figure 3: Documentary quality dropped 8.4% during 11x expansion (1990-2019): 3.57 (429 movies) → 3.27 (4,836 movies). Strong negative correlation (r = -0.937, p = 0.006) confirms quality degradation accompanies scaling._

#### Within-Genre Volume-Quality Correlations (1990-2019):

| Genre | Volume Growth | Quality Change | Correlation | p-value | Pattern |
|--------|---------|-------------|---------|----------|----------|
| **Documentary** | 11x (429→4,836) | -8.4% (3.57→3.27) | r=-0.937 | **0.006** | ✗ Constrain + curate |
| **Romance** | 3x (559→1,572) | -7.2% (2.90→2.69) | r=-0.934 | **0.006** | ✗ Constrain + curate |
| **Comedy** | 3x (1,024→3,646) | Stable | r=-0.471 | 0.345 | ✓ Scale confidently |
| **Thriller** | 3x (1,087→3,228) | Stable | r=-0.438 | 0.385 | ✓ Scale confidently |
| **Crime** | Expansion | Linear decline | p=0.002 | **0.002** | Prestige projects only |

![Volume Quality Relationships](images/volume_quality_relationships.png)
_Figure 4: Genre-specific scaling patterns reveal differentiated risks: Documentary and Romance show statistically significant quality degradation (r ≈ -0.93, p = 0.006), while Comedy and Thriller maintain quality despite similar growth rates._

**Key insight:** High-baseline-quality genres (Documentary: 3.57 → 3.27) face greater scaling vulnerability than moderate-quality genres (Comedy: stable ~2.7-2.8). Quality erosion is not universal—it depends on genre-specific curation capacity and audience expectations.

## Finding #3: Engagement is Genre-Specific, Not Metric-Driven

![Engagement Analysis](images/top_12_genres_critique_volume_vs_average_engagement_quality.png)
_Figure 5: Drama achieves both high volume (160,723 critiques) and high engagement (0.79 avg likes). Comedy shows high volume (51,426) but lowest engagement (0.63), while Horror maintains high engagement with low volume—demonstrating engagement is genre-specific, not scale-driven._

#### Engagement Analysis Results:

- Looking at all movies at once: no volume-quality relationship (p = 0.47)
- Conclusion: "Scaling doesn't affect quality"

**Temporal View (Reveals Truth):**
- Looking at genres over time: significant quality decline during scaling
- Documentary: 85% of films produced 1990-2019 when quality dropped 8.4%
- Conclusion: "Aggregate metrics mask temporal degradation"

**Implication:** Strategic decisions based solely on cross-sectional data can be flawed.

---

## 📈 Key Findings

### **Finding 1: Genre-Specific Scaling Vulnerability**

**High Baseline Quality Genres Struggle Most:**

| Genre | Avg Rating | 1990-2019 Trend | Statistical Significance |
|-------|-----------|-----------------|-------------------------|
| **Documentary** | 3.37 | 3.57 → 3.27 (-8.4%) | p = 0.006, r = -0.937 ⭐ |
| **Romance** | 2.92 | Declining | p < 0.05 ⭐ |
| **Comedy** | 2.84 | Stable | p > 0.05 ✅ |
| **Thriller** | 3.06 | Stable | p > 0.05 ✅ |


### **Finding 2: The Cross-Sectional Paradox**

**Why Aggregate Analysis Failed:**
1. 85% of Documentary films produced during declining period (1990-2019)
2. Cross-sectional average (3.37) represents mostly low-quality period
3. Earlier high-quality period (3.57) underweighted due to lower volume
4. Result: Aggregate hides the decline that temporal analysis reveals

### **Finding 3: Genre Dominance in Dataset**

| Genre | Movie Count | Avg Rating | Popularity |
|-------|------------|------------|------------|
| Drama | 45,765 (20.2%) | 3.05 | High |
| Comedy | 24,018 (10.6%) | 2.84 | High |
| Documentary | 14,509 (6.4%) | 3.37 | Medium |
| Thriller | 13,682 (6.0%) | 3.06 | High |

---

## 💡 Strategic Recommendations

### **For Content Platforms:**

**1. Genre-Specific Scaling Strategies**
- **Constrain expansion:** Documentary and Romance (vulnerable to quality loss)
- **Scale aggressively:** Comedy and Thriller (maintain quality during growth)

**2. Quality Monitoring Framework**
- Implement temporal tracking (not just cross-sectional snapshots)
- Monitor within-genre trends during scaling periods
- Early warning system: quality decline → pause expansion

**3. Resource Allocation**
- High-baseline genres need premium curation during scaling
- Lower-baseline genres can absorb volume better

**4. Methodological Lesson**
- Always combine cross-sectional AND temporal analysis
- Don't rely solely on aggregate metrics
- Simpson's Paradox awareness in strategic planning

---
