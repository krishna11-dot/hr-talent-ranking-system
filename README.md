# hr-talent-ranking-system

A Google Colab-based intelligent system for identifying, ranking, and re-ranking potential HR candidates using NLP and machine learning techniques.

## Overview
This project provides an automated talent ranking system implemented in Google Colab for processing HR candidate data. It uses natural language processing and machine learning to evaluate and rank candidates based on their job titles, locations, and professional connections.

## Quick Start
1. Open the notebook in Google Colab:
2. Upload your `potential_talents.xlsx` file
3. Run all cells
4. Follow the interactive starring system to refine rankings

## Input Data Format
The system expects an Excel file (`potential_talents.xlsx`) with the following columns:
- `id`: Unique identifier for each candidate
- `job_title`: Current or desired job title
- `location`: Geographic location
- `connection`: Number of connections (e.g., "500+")

## Features
- Automated candidate preprocessing
- Job title standardization
- Location normalization
- Connection strength analysis
- Interactive candidate starring
- Dynamic re-ranking

## How It Works
1. **Data Preprocessing**
   - Cleans and standardizes job titles
   - Normalizes location names
   - Processes connection counts

2. **Scoring System**
   - Title similarity (85% weight)
   - Clustering score (15% weight)
   - Connection strength modifier

3. **Re-ranking System**
   - Star-based score adjustments
   - Similar profile boosting
   - Location-based considerations

## Example Usage
```python
# Initialize processor
processor = HRDataProcessor()

# Upload and process data
from google.colab import files
uploaded = files.upload()  # Upload potential_talents.xlsx
df = pd.read_excel('potential_talents.xlsx')
results = processor.process_data(df)

# Star a candidate to re-rank
processor.star_candidate(results.iloc[6]['id'])
