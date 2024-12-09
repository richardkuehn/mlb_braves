# Atlanta Braves - Data Developer Exercises
#### Richard 'Ricky' Kuehn
#### December 8th, 2024
This repository contains two Jupyter notebooks that analyze baseball data for the Atlanta Braves using SQL queries and the MLB Stats API.

## SQL Analysis (`rkuehn_braves_SQL.ipynb`)
### Setup
- Uses SQLite3 and pandas for data analysis
- Connects to provided SQLite database 'main'
- Works with three tables:
  - WAR: Yearly batting WAR values (2000-2005)
  - PERF: Aggregated stint-level pitching performance
  - PITCHBYPITCH: Detailed pitch-level data

### Queries

1. **WAR Analysis (2002-2003)**
   - Lists all batters that had a single season WAR greater than 3 during the 2002 or 2003 seasons
   - Lists batters with combined WAR greater than 5 over those two seasons
   - Results listed in descending order of their combined WAR for those seasons
   - Uses CTE for readability and efficiency

2. **Braves Pitchers WAR Classification (2018)**
   - Returns every pitcher who threw at least one pitch for the Atlanta Braves in 2018
   - Creates three 1/0 indicator columns for whether they reached:
     - 1+ WAR cutoff
     - 2+ WAR cutoff
     - 3+ WAR cutoff
   - Includes a fourth column for their total yearly WAR

3. **Luke Jackson Pitch Analysis**
   - Calculates plate appearances that reached a two-strike count but did NOT result in a strikeout
   - Of those plate appearances, determines how many passed through:
     - 0-2 count
     - 1-2 count
     - 2-2 count

### Output Formats
Each query is originally returned as a tuple, converted to a dataframe, and exported as a csv.


## API Analysis (`rkuehn_braves_API.ipynb`)
### Setup
- Uses requests, pandas, and json libraries
- Connects to MLB Stats API endpoint for 2018 Braves pitching stats
- References local SQLite database for comparison

### Features

1. **Data Collection**
   - Fetches pitching statistics from MLB Stats API
   - Processes JSON response into pandas DataFrame
   - Includes key stats like games, innings pitched, hits, strikeouts

2. **Data Transformation**
   - Aggregates pitch-by-pitch data at various levels
   - Creates comparable metrics between API and local database
   - Maintains consistent data types for analysis

### Key Findings
When comparing the StatsAPI feed with the PITCHBYPITCH table:

1. **Data Coverage**
   - PITCHBYPITCH table contains partial season data for selected pitchers
   - API provides complete season statistics for all pitchers

2. **Metric Alignment**
   - Hit counts match between sources for tracked players
   - Strikeout totals align within expected variance
   - Pitch counts are consistently tracked

3. **Potential Audit System**
   - Could implement automated comparisons of key metrics
   - Should track discrepancies in:
     - Games pitched
     - Total pitches
     - Hit classifications
     - Strikeout counts

## Running the Code

1. Install required packages:
```bash
pip install pandas sqlite3 requests jupyter
```

2. Ensure database file 'main' is in working directory
