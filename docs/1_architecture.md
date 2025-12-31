# System Architecture Overview

## High-Level Architecture

Your HR Talent Ranking System is a **multi-agent machine learning pipeline** that combines several AI techniques to rank job candidates intelligently.

```
┌─────────────────────────────────────────────────────────────────┐
│                      INPUT LAYER                                │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │  Raw Candidate Data (CSV)                                │   │
│  │  - Job Title                                             │   │
│  │  - Location                                              │   │
│  │  - Connections                                           │   │
│  │  - ID                                                    │   │
│  └──────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────────┐
│                   DATA PROCESSING LAYER                         │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │  HRTalentProcessor                                       │   │
│  │  ┌────────────────┐  ┌────────────────┐  ┌────────────┐ │   │
│  │  │ Title Cleaner  │  │ Location       │  │ Connection │ │   │
│  │  │                │  │ Standardizer   │  │ Normalizer │ │   │
│  │  └────────────────┘  └────────────────┘  └────────────┘ │   │
│  │                                                          │   │
│  │  Output:                                                │   │
│  │  - cleaned_title: "aspiring human resources manager"    │   │
│  │  - cleaned_location: "texas"                            │   │
│  │  - cleaned_connections: 250                             │   │
│  └──────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────────┐
│                   SCORING LAYER (Multi-Agent)                   │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │  7 Ranking Agents with Weighted Evaluation:             │   │
│  │                                                          │   │
│  │  ┌──────────────────────────────────────────────────┐   │   │
│  │  │ Title Agents (3 agents × 30% = 90% total)       │   │   │
│  │  │ - Agent 1: Evaluates job title relevance        │   │   │
│  │  │ - Agent 2: Evaluates job title relevance        │   │   │
│  │  │ - Agent 3: Evaluates job title relevance        │   │   │
│  │  └──────────────────────────────────────────────────┘   │   │
│  │                                                          │   │
│  │  ┌──────────────────────────────────────────────────┐   │   │
│  │  │ Location Agents (2 agents × 2.5% = 5% total)    │   │   │
│  │  │ - Agent 4: Scores based on tech hub tier        │   │   │
│  │  │ - Agent 5: Scores based on tech hub tier        │   │   │
│  │  └──────────────────────────────────────────────────┘   │   │
│  │                                                          │   │
│  │  ┌──────────────────────────────────────────────────┐   │   │
│  │  │ Network Agents (2 agents × 2.5% = 5% total)     │   │   │
│  │  │ - Agent 6: Network size evaluation              │   │   │
│  │  │ - Agent 7: Network size evaluation              │   │   │
│  │  └──────────────────────────────────────────────────┘   │   │
│  │                                                          │   │
│  │  Final Score = Σ(Agent Score × Agent Weight)            │   │
│  └──────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────────┐
│                   CLUSTERING LAYER                              │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │  TalentClusterer                                         │   │
│  │                                                          │   │
│  │  Step 1: TF-IDF Vectorization                           │   │
│  │  ┌────────────────────────────────────────────────┐     │   │
│  │  │ "HR Manager" → [0.2, 0.8, 0.1, ...]          │     │   │
│  │  │ "HR Specialist" → [0.3, 0.7, 0.2, ...]       │     │   │
│  │  └────────────────────────────────────────────────┘     │   │
│  │                                                          │   │
│  │  Step 2: K-Means Clustering (5 clusters)                │   │
│  │  ┌────────────────────────────────────────────────┐     │   │
│  │  │ Cluster 0: Aspiring HR roles                  │     │   │
│  │  │ Cluster 1: Senior HR executives               │     │   │
│  │  │ Cluster 2: Mid-level HR specialists           │     │   │
│  │  │ Cluster 3: Entry-level HR coordinators        │     │   │
│  │  │ Cluster 4: Non-HR roles                       │     │   │
│  │  └────────────────────────────────────────────────┘     │   │
│  └──────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────────┐
│                   BIAS PREVENTION LAYER                         │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │  Within each cluster:                                    │   │
│  │  1. Normalize scores (z-score normalization)             │   │
│  │  2. Apply diversity factor (±10%)                        │   │
│  │  3. Apply keyword representation adjustment (±10%)       │   │
│  │                                                          │   │
│  │  Purpose: Prevent one cluster from dominating rankings   │   │
│  └──────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────────┐
│                   TIE-BREAKING LAYER                            │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │  GeneticTieBreaker                                       │   │
│  │                                                          │   │
│  │  When candidates have identical scores:                 │   │
│  │                                                          │   │
│  │  ┌────────────────────────────────────────────────┐     │   │
│  │  │ Generation 1: Create 50 random orderings      │     │   │
│  │  │ ↓                                             │     │   │
│  │  │ Evaluate fitness of each ordering             │     │   │
│  │  │ ↓                                             │     │   │
│  │  │ Select best orderings as "parents"            │     │   │
│  │  │ ↓                                             │     │   │
│  │  │ Crossover: Combine parent orderings           │     │   │
│  │  │ ↓                                             │     │   │
│  │  │ Mutation: Randomly swap some positions        │     │   │
│  │  │ ↓                                             │     │   │
│  │  │ Generation 2...Generation 10                  │     │   │
│  │  │ ↓                                             │     │   │
│  │  │ Return best ordering from all generations     │     │   │
│  │  └────────────────────────────────────────────────┘     │   │
│  └──────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────────┐
│                   OPTIONAL: RERANKING LAYER                     │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │  When an expert "stars" a candidate:                     │   │
│  │                                                          │   │
│  │  Calculate similarity to starred candidate:             │   │
│  │  - Role type match (40%): same job level?               │   │
│  │  - Experience match (30%): same seniority?              │   │
│  │  - Title similarity (30%): keyword overlap?             │   │
│  │  - Location bonus (+5%): same location?                 │   │
│  │                                                          │   │
│  │  Re-sort all candidates by new similarity scores         │   │
│  └──────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────────┐
│                      OUTPUT LAYER                               │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │  Ranked Candidate List                                   │   │
│  │                                                          │   │
│  │  Rank #1: [ID] [Title] [Score]                          │   │
│  │  Rank #2: [ID] [Title] [Score]                          │   │
│  │  Rank #3: [ID] [Title] [Score]                          │   │
│  │  ...                                                     │   │
│  │                                                          │   │
│  │  Categories:                                             │   │
│  │  - Primary Targets (Score ≥ 0.90)                       │   │
│  │  - Senior HR (Score 0.40-0.89)                          │   │
│  │  - Other Roles (Score < 0.40)                           │   │
│  └──────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────┘
```

## Component Interactions

### 1. Data Flow Through the System

```
CSV File
   ↓
[Load & Validate]
   ↓
[Clean Title] → "aspiring human resources manager"
[Clean Location] → "texas"
[Clean Connections] → 250
   ↓
[Multi-Agent Scoring]
   Agent 1 (Title): 0.92 × 0.30 = 0.276
   Agent 2 (Title): 0.92 × 0.30 = 0.276
   Agent 3 (Title): 0.92 × 0.30 = 0.276
   Agent 4 (Location): 1.00 × 0.025 = 0.025  (Texas = Tier 1)
   Agent 5 (Location): 1.00 × 0.025 = 0.025
   Agent 6 (Network): 0.50 × 0.025 = 0.0125  (250/500 = 0.5)
   Agent 7 (Network): 0.50 × 0.025 = 0.0125
   ────────────────────────────────────────
   Consensus Score: 0.90
   ↓
[Clustering] → Cluster 0 (Aspiring HR)
   ↓
[Bias Prevention] → Adjusted Score: 0.92
   ↓
[If ties exist] → Genetic Tie-Breaking
   ↓
[Output] → Rank #12, Score: 0.92
```

## Architecture Patterns Used

### 1. Multi-Agent System
- **Pattern**: Multiple specialized agents vote on candidate quality
- **Benefit**: Robustness through diverse perspectives
- **Implementation**: 7 agents with weighted voting

### 2. Ensemble Learning
- **Pattern**: Combine multiple evaluations for final decision
- **Benefit**: More accurate than single method
- **Implementation**: Weighted average of agent scores

### 3. Unsupervised Learning
- **Pattern**: K-Means clustering without labeled training data
- **Benefit**: Discovers natural groupings in data
- **Implementation**: 5 clusters based on title similarity

### 4. Evolutionary Algorithm
- **Pattern**: Genetic algorithm for optimization
- **Benefit**: Finds optimal ordering when scores are equal
- **Implementation**: 50 population, 10 generations

### 5. Feedback Loop
- **Pattern**: System learns from expert selections
- **Benefit**: Adapts to user preferences
- **Implementation**: Reranking based on starred candidates

## Key Design Principles

### 1. Separation of Concerns
Each component has a single responsibility:
- `GeneticTieBreaker` → Only handles ties
- `TalentClusterer` → Only handles grouping
- `RankingAgent` → Only handles one scoring criterion
- `HRTalentProcessor` → Only orchestrates the pipeline

### 2. Modularity
Components can be modified independently:
- Change clustering algorithm without affecting scoring
- Adjust agent weights without changing tie-breaking
- Modify title cleaning without impacting other cleaners

### 3. Configurability
Key parameters are adjustable:
- Number of clusters (default: 5)
- Agent weights (Title: 90%, Location: 5%, Network: 5%)
- Genetic algorithm settings (population: 50, generations: 10)
- Mutation rate (default: 0.1)

### 4. Robustness
Multiple fallbacks for errors:
- Try-except blocks in all critical methods
- Logging for debugging
- Default values when data is missing
- Graceful degradation (e.g., skip clustering if < 5 candidates)

## Data Structures

### Main DataFrame Schema

```python
{
    # Original fields
    'id': int,              # Unique candidate identifier
    'job_title': str,       # Raw job title from input
    'location': str,        # Raw location from input
    'connection': str,      # Raw connection count (e.g., "500+")

    # Processed fields
    'cleaned_title': str,         # Standardized title category
    'cleaned_location': str,      # Standardized location
    'cleaned_connections': int,   # Normalized connection count (0-500)

    # Scoring fields
    'base_score': float,          # Initial score from title (0-1)
    'agent_score': float,         # Multi-agent consensus (0-1)
    'final_score': float,         # After bias prevention (0-1)

    # Clustering fields
    'cluster': int,                      # Cluster ID (0-4)
    'cluster_similarity': float,         # Similarity to cluster centroid (0-1)
    'cluster_normalized_score': float    # Z-score within cluster
}
```

## Performance Characteristics

### Time Complexity
- **Data Cleaning**: O(n) - linear scan through candidates
- **Multi-Agent Scoring**: O(n × m) where m=7 agents
- **TF-IDF Vectorization**: O(n × d) where d=100 features
- **K-Means Clustering**: O(n × k × i) where k=5 clusters, i=iterations
- **Genetic Tie-Breaking**: O(t × p × g) where t=tied candidates, p=50 population, g=10 generations
- **Overall**: O(n × m) dominated by agent scoring for typical datasets

### Space Complexity
- **TF-IDF Matrix**: O(n × 100) - sparse matrix storage
- **Agent Storage**: O(7) - constant
- **DataFrame**: O(n × c) where c=number of columns
- **Overall**: O(n) linear in number of candidates

### Scalability Considerations
- **Current Design**: Optimized for 100-1000 candidates
- **Bottleneck**: Genetic tie-breaking if many ties exist
- **Optimization Opportunity**: Vectorize agent scoring, use sparse matrices
- **Trade-off**: Accuracy vs. speed (can reduce population size or generations)
