# The passing stats page is a classic report page.
# SQL query for this report page is:

```sql
## Query: Player Passing Stats Report
## Purpose: This query retrieves player passing statistics for generating a report.
-- The report includes the player's ID, name, position, in-system passes, out-of-system passes,
-- and the percentage of good and bad passes based on the total number of passes.

SELECT
    p.PLAYER_ID,
    p.NAME,
    p.POSITION,
    p.CLASS,
    p.JERSEY_NUMBER,
    ps.IN_SYSTEM_PASSES,
    ps.OUT_OF_SYSTEM_PASSES,
    CASE WHEN (ps.IN_SYSTEM_PASSES + ps.OUT_OF_SYSTEM_PASSES) = 0 THEN NULL
        ELSE ROUND((ps.IN_SYSTEM_PASSES / (ps.IN_SYSTEM_PASSES + ps.OUT_OF_SYSTEM_PASSES)) * 100, 2)
    END AS PERCENT_GOOD,
    CASE WHEN (ps.IN_SYSTEM_PASSES + ps.OUT_OF_SYSTEM_PASSES) = 0 THEN NULL
        ELSE ROUND((ps.OUT_OF_SYSTEM_PASSES / (ps.IN_SYSTEM_PASSES + ps.OUT_OF_SYSTEM_PASSES)) * 100, 2)
    END AS PERCENT_BAD
FROM
    PLAYER p
JOIN
    PASSING_STATS ps ON p.PLAYER_ID = ps.PLAYER_ID;
