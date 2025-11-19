================================================================================
PROJECT TITLE: 2015-16 Golden State Warriors Performance Dashboard
AUTHOR: [Your Name]
DATE: [Current Date]
================================================================================

[ OVERVIEW ]
This repository contains an advanced SQL project designed to audit and visualize 
team performance stability. Using data from the historic 2015-2016 NBA season, 
this project simulates a business intelligence request to analyze individual 
contributor trends, establish team hierarchies, and provide contextual 
benchmarking for performance reviews.

[ BUSINESS PROBLEM ]
A stakeholder (The Coaching Staff) requested a "Shooting Stability Report" to 
better understand player consistency. Standard aggregate metrics (like season 
averages) hide volatility. The stakeholder needed a view that could answer:
1. Trend Analysis: Is a player's performance trending up or down compared to 
   previous events?
2. Hierarchy: Where does this player rank within the organization?
3. Context: For every game log, how far is this player from the team's 
   best and worst performers?

[ DATA SOURCE ]
The analysis utilizes a raw dataset (`player_stats.csv`) containing game-by-game 
logs for NBA players.
- Key Filters Applied: Team (Golden State Warriors), Season (2015-2016).
- Key Metrics Analyzed: 3-Point Field Goals Made ("3P").

[ TECHNICAL SKILLS DEMONSTRATED ]
This project moves beyond basic data retrieval to perform complex analytical 
transformations. Key SQL concepts include:

1. Common Table Expressions (CTEs): 
   Used to pre-calculate season-long baselines and clean the dataset before 
   ranking logic is applied.

2. Advanced Window Functions:
   - Chronological Sequencing: Using ROW_NUMBER() to create time-series 
     identifiers.
   - Trend Analysis: Using LAG() and LEAD() to fetch values from previous and 
     subsequent rows, enabling "period-over-period" analysis without self-joins.
   - Ranking Logic: Using DENSE_RANK() to handle ties in performance data 
     appropriately.

3. Absolute Fetching & Benchmarking:
   - Using FIRST_VALUE() and LAST_VALUE() to dynamically populate every row 
     with the team's "Ceiling" (Top Performer) and "Floor" (Lowest Performer) 
     for immediate contextual comparison.

[ PROJECT FILES ]
- `analysis_query.sql`: The complete SQL script containing the logic described above.
- `player_stats.csv`: The source dataset used for the query.
- `README.txt`: Project documentation.

[ HOW TO INTERPRET THE OUTPUT ]
The query generates a table where each row represents a single game for a player. 
However, unlike a standard box score, this output includes:
- `Game_Seq`: The chronological order of the game for that specific player.
- `Prev_Game_3P` / `Next_Game_3P`: Performance context from surrounding games.
- `Team_Rank`: The player's standing in the team hierarchy based on season averages.
- `Top_Shooter` / `Low_Shooter`: The names of the benchmarks used for comparison.

[ CONTACT ]
For questions regarding the query logic or methodology, please contact me via 
profile details provided in this repository.
