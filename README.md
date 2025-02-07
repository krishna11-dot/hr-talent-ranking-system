# HR Talent Ranking System

A machine learning system for ranking HR candidates using clustering, genetic algorithms, and expert feedback.

## Key Components

### Multi-Agent Evaluation
- Title Agents (90%): 3 agents × 30%
- Location Agents (5%): 2 agents × 2.5%
- Connection Agents (5%): 2 agents × 2.5%

### Score Thresholds
| Score | Category | Description |
|-------|----------|-------------|
| 0.90-1.00 | Primary Target | Aspiring/seeking HR roles |
| 0.70-0.89 | Senior HR | Leadership positions |
| 0.50-0.69 | Standard HR | Specialists/Managers |
| 0.40-0.49 | HR Adjacent | Related roles |
| 0.05-0.39 | Non-HR | Other positions |

### Features
- TF-IDF title vectorization
- K-means clustering (n=5)
- Genetic tie-breaking
- Bias prevention through cluster normalization

## Installation
```python
!pip install fuzzywuzzy python-Levenshtein scikit-learn pandas numpy
```

## Usage

1. Load data:
```python
df = pd.read_csv('potential-talents.csv')
```

2. Initialize processor:
```python
processor = HRTalentProcessor()
results = processor.process_data(df)
```

3. Display rankings:
```python
processor.display_results(results)
```

4. Re-rank based on starred candidate:
```python
updated_results = processor.rerank_after_starring(results, starred_id=7)
```

## Output Format
```
CANDIDATE DISTRIBUTION
---------------------
Target Roles:  47.1%
Senior HR:     26.0%
Other Roles:   26.9%

CANDIDATE DETAILS
----------------
ID:        {id}
Title:     {job_title}
Category:  {cleaned_title}
Location:  {cleaned_location}
Network:   {cleaned_connections}
Score:     {final_score:.3f}
```

## Classes
- `HRTalentProcessor`: Main ranking system
- `TalentClusterer`: Title-based clustering
- `GeneticTieBreaker`: Resolves ranking ties
- `RankingAgent`: Individual scoring agents

## Tech Stack
- pandas/numpy: Data processing
- scikit-learn: ML components
- TF-IDF: Text vectorization
- K-means: Clustering
- Genetic algorithms: Tie-breaking
