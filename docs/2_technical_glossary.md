# Technical Glossary - Simple Explanations

## Machine Learning & AI Concepts

### TF-IDF (Term Frequency-Inverse Document Frequency)
**What it is**: A way to convert text into numbers so computers can understand and compare it.

**Simple analogy**: Imagine you're reading job titles. Words like "the" and "a" appear everywhere, so they're not useful. But "aspiring" or "specialist" are more rare and tell you something important about the role.

**How it works**:
1. **TF (Term Frequency)**: How often does a word appear in this specific title?
   - Example: "HR Manager at HR Department" → "HR" appears 2 times
2. **IDF (Inverse Document Frequency)**: How rare is this word across ALL titles?
   - If "HR" appears in 90% of titles → low IDF (not distinctive)
   - If "aspiring" appears in 10% of titles → high IDF (very distinctive)
3. **TF-IDF Score** = TF × IDF
   - Common words get low scores
   - Distinctive words get high scores

**Example**:
```
Title: "Aspiring HR Manager"

Word        TF    IDF    TF-IDF
aspiring    1  ×  3.2  =  3.2   (rare, important!)
hr          1  ×  0.5  =  0.5   (common, less important)
manager     1  ×  1.2  =  1.2   (somewhat common)

Vector: [3.2, 0.5, 1.2, ...]
```

### K-Means Clustering
**What it is**: An algorithm that automatically groups similar items together.

**Simple analogy**: Imagine you have 100 colored marbles mixed together. K-Means is like having 5 bins and automatically sorting marbles into bins by color similarity.

**How it works**:
1. **Choose K**: Decide how many groups you want (e.g., 5 clusters)
2. **Random Start**: Place 5 "cluster centers" randomly
3. **Assign**: Each candidate goes to their nearest cluster center
4. **Update**: Move cluster centers to the middle of their groups
5. **Repeat**: Steps 3-4 until groups stop changing

**Example with job titles**:
```
Initial: Random 5 centers

Iteration 1:
Cluster 0: "Aspiring HR", "Seeking HR", "HR Student"
Cluster 1: "VP HR", "Director HR", "CHRO"
Cluster 2: "HR Specialist", "HR Manager", "HR Generalist"
Cluster 3: "HR Coordinator", "HR Assistant"
Cluster 4: "Teacher", "Engineer", "Student"

After 10 iterations: Clusters stabilize
```

### Cosine Similarity
**What it is**: A measure of how similar two things are, ranging from 0 (completely different) to 1 (identical).

**Simple analogy**: Think of two job titles as arrows pointing in different directions. If they point in the same direction, they're similar. If they point in opposite directions, they're different.

**How it works**:
```
Title 1: "HR Manager"        → Vector: [0.8, 0.2, 0.1]
Title 2: "HR Specialist"     → Vector: [0.7, 0.4, 0.1]
Title 3: "Sales Engineer"    → Vector: [0.1, 0.1, 0.9]

Cosine Similarity:
Title 1 vs Title 2 = 0.85  (very similar!)
Title 1 vs Title 3 = 0.15  (very different!)
```

**Math (simplified)**:
- Take dot product of vectors
- Divide by vector lengths
- Result is between 0 and 1

### Genetic Algorithm
**What it is**: An optimization technique inspired by biological evolution.

**Simple analogy**: You're trying to breed the "best" dog. You:
1. Start with 50 random dogs
2. Let the best dogs have puppies
3. Sometimes puppies have random mutations
4. Repeat for 10 generations
5. End up with much better dogs

**How it works in our system**:
1. **Population**: 50 different candidate rankings
2. **Fitness**: Score each ranking by how well it separates candidates
3. **Selection**: Choose best rankings as "parents"
4. **Crossover**: Combine two parent rankings to create "child" rankings
5. **Mutation**: Randomly swap positions with 10% probability
6. **Repeat**: For 10 generations
7. **Result**: Best ranking found across all generations

**Example**:
```
Generation 1:
Ranking A: [Candidate 5, Candidate 3, Candidate 1] → Fitness: 0.75
Ranking B: [Candidate 3, Candidate 5, Candidate 1] → Fitness: 0.82
Ranking C: [Candidate 1, Candidate 5, Candidate 3] → Fitness: 0.68

Select B as parent (highest fitness)

Generation 2:
Create children by crossing over B with other rankings
Mutate some positions randomly

Generation 10:
Best ranking: [Candidate 3, Candidate 5, Candidate 1] → Fitness: 0.94
```

### Multi-Agent System
**What it is**: Multiple "agents" (like team members) that each specialize in evaluating one aspect, then vote on the final decision.

**Simple analogy**: Hiring committee where:
- Agent 1 evaluates work experience
- Agent 2 evaluates education
- Agent 3 evaluates skills
- Agent 4 evaluates cultural fit
- Final decision = weighted average of all votes

**How it works**:
```
Candidate: John Smith - "HR Manager in Texas with 300 connections"

Agent 1 (Title Expert):    Score = 0.65, Weight = 30% → 0.195
Agent 2 (Title Expert):    Score = 0.65, Weight = 30% → 0.195
Agent 3 (Title Expert):    Score = 0.65, Weight = 30% → 0.195
Agent 4 (Location Expert): Score = 1.00, Weight = 2.5% → 0.025  (Texas is Tier 1!)
Agent 5 (Location Expert): Score = 1.00, Weight = 2.5% → 0.025
Agent 6 (Network Expert):  Score = 0.60, Weight = 2.5% → 0.015  (300/500 = 0.6)
Agent 7 (Network Expert):  Score = 0.60, Weight = 2.5% → 0.015
                                                         ─────
Final Consensus Score:                                   0.665
```

### Z-Score Normalization
**What it is**: A way to standardize scores so they're comparable, even if they come from different ranges.

**Simple analogy**: Converting test scores from different exams to the same scale. An 80% on an easy test might be worth the same as a 60% on a hard test.

**How it works**:
1. **Calculate mean** of all scores in a group
2. **Calculate standard deviation** (how spread out scores are)
3. **Transform each score**: (score - mean) / std_dev

**Example**:
```
Cluster 0 candidates (Aspiring HR):
Scores: [0.90, 0.92, 0.91, 0.93, 0.90]
Mean = 0.912
Std Dev = 0.012

Candidate with 0.93:
Z-score = (0.93 - 0.912) / 0.012 = 1.5

Interpretation: This candidate is 1.5 standard deviations ABOVE average
```

## Domain-Specific Terms

### Tech Hub Tiers
**What it is**: Classification of locations by their importance in the tech/business world.

**Why it matters**: Candidates in major tech hubs might have better networks, more experience, or easier relocation.

**Our tiers**:
```
Tier 1 (Score: 1.0 = 100%):
- Texas (Austin, Houston, Dallas)
- California (San Francisco, LA)
- New York

Tier 2 (Score: 0.7 = 70%):
- Illinois (Chicago)
- Massachusetts (Boston)
- North Carolina (Research Triangle)

Focus Areas (Score: 0.6 = 60%):
- Canada
- Turkey

Other (Score: 0.3 = 30%):
- All other locations
```

### Aspiring vs. Seeking
**Aspiring**: Someone who wants to enter a field (e.g., "Aspiring HR Professional")
- Usually students or career changers
- High potential but low experience
- Our PRIMARY TARGET (Score: 0.90-0.95)

**Seeking**: Someone actively looking for a position (e.g., "Seeking HR Opportunities")
- May have some experience
- Actively job hunting
- Also a PRIMARY TARGET (Score: 0.90-0.95)

### Role Categorization

```
┌─────────────────────────────────────────────────────────┐
│ PRIMARY TARGETS (Score: 0.90-1.00)                      │
│ - Aspiring HR Professional/Specialist/Manager           │
│ - Seeking HR Position/Opportunities                     │
│ WHY: High potential, trainable, eager to learn          │
└─────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────┐
│ SENIOR HR (Score: 0.70-0.89)                            │
│ - Chief Human Resources Officer (CHRO)                  │
│ - SVP/Director of HR                                    │
│ - Senior HR Specialist                                  │
│ WHY: Experienced but expensive, may be overqualified    │
└─────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────┐
│ MID-LEVEL HR (Score: 0.50-0.69)                         │
│ - HR Manager                                            │
│ - HR Specialist                                         │
│ - HR Generalist                                         │
│ WHY: Solid experience, may lack specific skills         │
└─────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────┐
│ JUNIOR HR (Score: 0.40-0.49)                            │
│ - HR Coordinator                                        │
│ - HR Assistant                                          │
│ WHY: Entry-level, needs training                        │
└─────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────┐
│ NON-HR ROLES (Score: 0.05-0.39)                         │
│ - Teachers, Engineers, Students, etc.                   │
│ WHY: Not relevant to HR positions                       │
└─────────────────────────────────────────────────────────┘
```

## Algorithmic Terms

### Fitness Function
**What it is**: A way to score how "good" a solution is.

**Simple analogy**: Like a report card grade. The fitness function evaluates how well a candidate ranking performs and gives it a score.

**In our system**:
```python
def fitness(candidate):
    # Evaluate candidate using all agents
    scores = []
    for agent in agents:
        score = agent.evaluate(candidate) * agent.weight
        scores.append(score)

    return sum(scores)  # Higher = better candidate
```

### Crossover
**What it is**: In genetic algorithms, combining two "parent" solutions to create a "child" solution.

**Simple analogy**: Like mixing genes from two parents to create a baby.

**Example**:
```
Parent 1 ranking: [A, B, C, D, E]
Parent 2 ranking: [D, E, A, B, C]

Crossover at position 3:
Child ranking: [A, B, C, B, C]  ← Take first 3 from P1, rest from P2
```

### Mutation
**What it is**: Random changes to introduce variety and avoid getting stuck.

**Simple analogy**: Like a random genetic mutation in biology. Most mutations are neutral or bad, but some can be beneficial.

**Example**:
```
Original ranking: [A, B, C, D, E]

Mutation (swap positions 2 and 4):
New ranking: [A, D, C, B, E]
             ↑     ↑
             swapped!
```

### Weighted Average
**What it is**: An average where some items count more than others.

**Simple analogy**: Your final grade where exams count 60% and homework counts 40%.

**Example**:
```
Regular average:
(90 + 70 + 80) / 3 = 80

Weighted average (weights: 50%, 30%, 20%):
(90 × 0.50) + (70 × 0.30) + (80 × 0.20)
= 45 + 21 + 16
= 82
```

### Consensus Score
**What it is**: The combined opinion of all agents, weighted by their importance.

**Simple analogy**: Like a jury vote where each member's vote might count differently based on their expertise.

**In our system**:
```
Title agents (very important): 90% of final score
Location agents (less important): 5% of final score
Network agents (less important): 5% of final score

Consensus = 0.90 × title_score + 0.05 × location_score + 0.05 × network_score
```

## Data Processing Terms

### Data Cleaning
**What it is**: Fixing messy or inconsistent data so it's usable.

**Examples**:
```
BEFORE cleaning:
- "500+" connections → AFTER: 500
- "Greater San Francisco Bay Area" → AFTER: "california"
- "Human Resources Manager" → AFTER: "human resources manager"

WHY:
- Standardize formats
- Remove noise
- Make comparisons possible
```

### Normalization
**What it is**: Scaling values to a standard range (usually 0 to 1).

**Simple analogy**: Converting different currencies to USD so you can compare prices.

**Example**:
```
BEFORE normalization:
Connections: 50, 300, 500+
Scores: 0.4, 0.7, 0.95

AFTER normalization (connections → 0-1 scale):
50 connections → 0.10  (50/500)
300 connections → 0.60  (300/500)
500 connections → 1.00  (500/500)

Now all values are 0-1 and comparable!
```

### Feature Engineering
**What it is**: Creating new useful variables from existing data.

**Example**:
```
Raw data:
- job_title: "Aspiring HR Manager"

Engineered features:
- cleaned_title: "aspiring human resources manager"
- is_aspiring: True
- is_manager_level: True
- title_category: "Primary Target"
- base_score: 0.92
```

## Bias Prevention Terms

### Cluster Normalization
**What it is**: Making sure one group doesn't dominate rankings just because it's larger.

**Problem example**:
```
WITHOUT cluster normalization:
Cluster 0 (50 candidates): Average score 0.85
Cluster 1 (5 candidates): Average score 0.90

All top 10 spots might go to Cluster 0 just because it's bigger!

WITH cluster normalization:
Scores are normalized within each cluster first
→ Top candidates from BOTH clusters get fair representation
```

### Diversity Factor
**What it is**: A small score boost for candidates in smaller clusters to ensure diversity.

**Why**: Prevents one large cluster from dominating all top positions.

**Calculation**:
```
Cluster sizes:
Cluster 0: 50 candidates (50/104 = 48%)
Cluster 1: 10 candidates (10/104 = 10%)

Diversity factor:
Cluster 0: 1 - 0.48 = 0.52 → 5.2% adjustment
Cluster 1: 1 - 0.10 = 0.90 → 9% adjustment

Cluster 1 candidates get a small boost for diversity
```

## Evaluation Metrics

### Precision vs. Recall (Conceptual)
Not directly used in code but important for understanding:

**Precision**: Of the candidates we rank highly, how many are actually good?
```
Top 10 candidates → 9 are truly "aspiring HR" → 90% precision
```

**Recall**: Of all the good candidates, how many did we rank highly?
```
20 total "aspiring HR" candidates → 9 in top 10 → 45% recall
```

### Score Distribution
**What it is**: How candidates are spread across different score ranges.

**Good distribution**:
```
0.90-1.00: 49 candidates (47%) ✓ Clear top tier
0.70-0.89: 15 candidates (14%)
0.40-0.69: 12 candidates (12%)
0.05-0.39: 28 candidates (27%) ✓ Clear bottom tier

This shows the system is making clear distinctions!
```

**Bad distribution**:
```
0.90-1.00: 5 candidates (5%)
0.85-0.90: 95 candidates (91%) ✗ Everyone bunched together!
0.00-0.85: 4 candidates (4%)

This would mean the system can't differentiate well.
```

## Quick Reference

### Scoring Summary
```
Title Analysis: 90% weight
├─ Aspiring HR: 0.90-0.95
├─ Seeking HR: 0.90-0.95
├─ Senior HR: 0.70-0.85
├─ Mid-level HR: 0.50-0.69
├─ Junior HR: 0.40-0.49
└─ Non-HR: 0.05

Location: 5% weight
├─ Tier 1 (TX, CA, NY): 1.0
├─ Tier 2 (IL, MA, NC): 0.7
├─ Focus (Canada, Turkey): 0.6
└─ Other: 0.3

Network: 5% weight
└─ Normalized: connections/500
```

### Component Cheat Sheet
```
GeneticTieBreaker
├─ Purpose: Break ties when scores are equal
├─ Method: Evolutionary optimization
└─ Settings: 50 population, 10 generations, 10% mutation

TalentClusterer
├─ Purpose: Group similar candidates
├─ Method: TF-IDF + K-Means
└─ Settings: 5 clusters, 100 features, 1-2 word n-grams

RankingAgent
├─ Purpose: Evaluate one aspect of candidates
├─ Types: Title (×3), Location (×2), Network (×2)
└─ Output: Score 0-1, weighted in consensus

HRTalentProcessor
├─ Purpose: Orchestrate entire pipeline
├─ Steps: Clean → Score → Cluster → Prevent Bias → Output
└─ Optional: Rerank based on starred candidate
```
