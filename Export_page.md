# The passing stats page is a classic report page.
# SQL query for this report page is:

# Query: Player Passing Stats Report
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
