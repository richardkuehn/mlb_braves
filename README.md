# Atlanta Braves - Data Developer Exercises
#### Richard 'Ricky' Kuehn
#### December 8th, 2024

This repository contains two Jupyter notebooks that analyze baseball data for the Atlanta Braves using SQL queries and the MLB Stats API.

***NOTE: This repository does not contain the SQLite database 'main'***

## Running the Code

1. Install required packages:
```bash
pip install pandas sqlite3 requests jupyter
```

2. Ensure database file 'main' is in working directory

## SQL Analysis (`rkuehn_braves_SQL.ipynb`)

### Database Structure
Works with three tables in the SQLite database 'main':
- WAR: Yearly batting WAR values (2000-2005)
- PERF: Aggregated stint-level pitching performance
- PITCHBYPITCH: Detailed pitch-level data with flags for various outcomes

### Implemented Queries

1. **Historical WAR Analysis (2002-2003)**
   - Lists batters with:
     - Single season WAR > 3 in 2002 or 2003
     - Combined WAR > 5 across both seasons
   - Uses Common Table Expression (CTE) for data aggregation
   - Results sorted by descending combined WAR
   - Outputs player name, individual year WARs, and combined WAR

2. **2018 Braves Pitchers WAR Classification**
   - Identifies all pitchers who threw for Atlanta in 2018
   - Creates binary indicators for WAR thresholds:
     - 1+ WAR
     - 2+ WAR
     - 3+ WAR
   - Includes actual WAR value
   - Filters for MLB-level appearances only

3. **Luke Jackson Pitch Sequence Analysis**
   - Analyzes two-strike counts that didn't result in strikeouts
   - Tracks progression through specific counts:
     - 0-2 count instances
     - 1-2 count instances
     - 2-2 count instances
   - Uses subqueries to maintain pitch sequence integrity

## API Analysis (`rkuehn_braves_API.ipynb`)

### Data Collection
- Fetches 2018 Braves pitching statistics from MLB Stats API endpoint
- Processes JSON response into structured DataFrame
- Creates normalized dataset for comparison with local database

### Data Processing Steps
1. Initial API data retrieval and JSON parsing
2. DataFrame construction with consistent column naming
3. Calculation of derived statistics (e.g., balls from total pitches minus strikes)
4. Aggregation of pitch-by-pitch data at player and game levels
5. Creation of comparable metrics between data sources

### Key Metrics Tracked
- Games played
- Batters faced
- Total pitches
- Hit outcomes (singles, doubles, triples, home runs)
- Strikeouts
- Balls/strikes counts
- Pitch outcomes (swings/takes)

## Data Analysis Findings

When comparing the StatsAPI feed with the PITCHBYPITCH table, several interesting patterns (and potential solutions) emerged:

1. **Data Discrepancies**
   - PITCHBYPITCH data only contained data on three pitchers (Jonny Venters, Luke Jackson, Shane Carle)
   - Johnny Venter's numbers are missing the data from 22 games
   - Luke Jackson's numbers are equal for games, doubles, triples, homeruns, and strikeouts. But it is 1 off for hits, 4 off for outs, and significantly off for balls and strikes
   - Carle Shane's numbers are off by 1 for games, 59 for batters_faced, 15 for pitches, 13 for outs, and significantly off for balls and strikes.

2. **Statistical Variations**
   - outs/balls/strikes show systematic differences
   - PITCHBYPITCH has additional granularity with swing/take data

3. **Audit System Recommendations**
   - Implement automated daily comparisons of key metrics
   - Track rate statistics for validation (K%, BB%, HR/9)
   - Flag outliers in pitch counts and outcome distributions
   - Monitor game-by-game alignment for complete coverage
   - Create versioned snapshots for tracking data updates

The analysis suggests that while some core outcome data aligns well, there are systematic differences in how certain metrics are tracked or calculated between the API and main database.

## Output Files

Each analysis generates CSV files for further processing:
- SQL Analysis: q1.csv, q2.csv, q3.csv
- API Analysis: raw_api_data.csv, cleaned_api_data.csv, cleaned_main_data.csv
