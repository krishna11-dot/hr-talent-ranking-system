# HR Talent Ranking System (Google Colab)

NLP and ML-powered system for ranking HR talent with interactive feedback.

## Quick Start

1. Open the [Potential_Talents_Ranking_System.ipynb](link_to_your_colab) notebook
2. Upload your CSV file with columns:
   - `ID`: Candidate identifier
   - `Job Title`: Position title
   - `Location`: Geographic location
   - `Connection`: Network size

3. Run all cells

## System Overview

### Scoring Weights
- Title Analysis: 90%
- Location Score: 5%
- Network Size: 5%

### Candidate Categories
| Score | Category | Description |
|-------|----------|-------------|
| 0.90+ | Primary Target | Aspiring/Seeking HR |
| 0.40-0.89 | Senior HR | Experienced professionals |
| <0.40 | Other | Non-HR roles |

### Re-ranking System
Star any candidate to re-rank based on:
- Role similarity (40%)
- Experience level (30%)
- Title keywords (30%)

## Output Format
```
CANDIDATE DETAILS
----------------
ID:       {id}
Title:    {job_title}
Category: {category}
Location: {location}
Score:    {score}
```

## Dependencies
```python
!pip install pandas numpy scikit-learn fuzzywuzzy python-Levenshtein
```

## Sample Usage
```python
processor = HRTalentProcessor()
results = processor.process_data(df)
updated_results = processor.rerank_after_starring(results, starred_id=7)
```
