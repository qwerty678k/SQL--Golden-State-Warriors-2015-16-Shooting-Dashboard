/*
 * PORTFOLIO PROJECT: 2015-16 Team Performance Dashboard
 * GOAL: Analyze individual performance trends, establish a team hierarchy, 
 * and provide contextual benchmarks for every record.
 */

WITH GSW_Shooting_Stats AS (
  SELECT
    player,
    game_id,
    "3P",
    /* * METRIC CALCULATION: 
     * Calculate the season-long baseline (average) for each individual.
     * This allows us to compare daily performance against their standard level.
     */
    AVG("3P") OVER (PARTITION BY player) AS Avg_3P
  FROM
    Player_Stats
  WHERE
    team = 'GSW' -- Focus on the Golden State Warriors organization
    AND SUBSTR(game_id, 1, 4) = '1516' -- Filter for the 2015-2016 fiscal year/season
)

SELECT
  player,
  game_id,
  "3P",
  Avg_3P,
  /* * CHRONOLOGICAL SEQUENCING:
   * Assign a unique ID to each event (game) in chronological order.
   * This is essential for identifying streaks or time-based patterns.
   */
  ROW_NUMBER() OVER (PARTITION BY player ORDER BY game_id ASC) AS Game_Seq,
  
  /* * TREND ANALYSIS:
   * Fetch values from the previous event (LAG) and the subsequent event (LEAD).
   * This allows us to visualize month-over-month or game-over-game volatility.
   */
  LAG("3P") OVER (PARTITION BY player ORDER BY game_id ASC) AS Prev_Game_3P,
  LEAD("3P") OVER (PARTITION BY player ORDER BY game_id ASC) AS Next_Game_3P,
  
  /* * HIERARCHY ESTABLISHMENT:
   * Rank all individuals based on their overall season performance.
   * DENSE_RANK ensures that ties share the same rank and no rank numbers are skipped.
   */
  DENSE_RANK() OVER (ORDER BY Avg_3P DESC) AS Team_Rank,
  
  /* * CONTEXTUAL BENCHMARKING:
   * Dynamically populate every row with the top performer (FIRST_VALUE) 
   * and lowest performer (LAST_VALUE) in the group.
   * This allows instant comparison of any individual against the group's ceiling and floor.
   */
  FIRST_VALUE(player) OVER (
    ORDER BY Avg_3P DESC
  ) AS Top_Shooter,
  
  LAST_VALUE(player) OVER (
    ORDER BY Avg_3P DESC
    -- Define the window to look at the entire dataset to find the true last value
    RANGE BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING
  ) AS Low_Shooter

FROM
  GSW_Shooting_Stats
ORDER BY
  Team_Rank ASC,
  Game_Seq ASC;
