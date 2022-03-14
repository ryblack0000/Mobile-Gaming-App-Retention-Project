# SQL Queries for Mobile Game Player Retention Project

### Working File in Google Sheets for Visuals / Graphs / Tables:
https://docs.google.com/spreadsheets/d/1HmwNxyhtVUWkQ6unLLF1up1uMqjlreljLSHjGQ1IIpw/edit?usp=sharing

## Appendix 1: 'retention_rate' Table Creation Query

```sql
SELECT
  day_joined,
  new_player_count,
  retained_count,
  retained_count/new_player_count AS retention_rate
FROM (
  SELECT
    joined AS day_joined,
    COUNT(retained_status) AS retained_count
  FROM (
    SELECT
      player_id,
      joined,
      CASE
        WHEN MAX(days_between) > 30 THEN "retained"
      ELSE
      "not-retained"
    END
      AS retained_status
    FROM (
      SELECT
        player_info.player_id,
        player_info.joined,
        matches_info.day AS day_played,
        matches_info.match_id,
        (matches_info.day - player_info.joined) AS days_between
      FROM
        `ninth-botany-329514.gamecompanydata.player_info` AS player_info
      JOIN
        `ninth-botany-329514.gamecompanydata.matches_info` AS matches_info
      ON
        player_info.player_id=matches_info.player_id
      ORDER BY
        joined,
        player_id,
        day_played)
    GROUP BY
      player_id,
      joined
    ORDER BY
      joined,
      player_id)
  WHERE
    retained_status = "retained"
  GROUP BY
    day_joined)
JOIN (
  SELECT
    joined,
    COUNT(DISTINCT player_id) AS new_player_count
  FROM
    `ninth-botany-329514.gamecompanydata.player_info`
  GROUP BY
    joined)
ON
  day_joined = joined
```

### Appendix 2: 'retention_status' Table Creation Query

```sql
SELECT
day_joined,
player_id,
days_elapsed_playing,
CASE WHEN days_elapsed_playing<=30 THEN 'not-retained' ELSE 'retained' END AS retention_status
FROM 
(
SELECT
player_id,
day_joined,
MAX(days_between) AS days_elapsed_playing
FROM (
  SELECT
    player_id,
    joined AS day_joined,
    day_played,
    days_between
  FROM (
    SELECT
      player_info.player_id,
      player_info.joined,
      matches_info.day AS day_played,
      (matches_info.day - player_info.joined) AS days_between
    FROM
      `ninth-botany-329514.gamecompanydata.player_info` AS player_info
    JOIN
      `ninth-botany-329514.gamecompanydata.matches_info` AS matches_info
    ON
      player_info.player_id=matches_info.player_id
    ORDER BY
      joined,
      player_id,
      day_played) )
GROUP BY player_id, day_joined)
ORDER BY day_joined, days_elapsed_playing
```

### Appendix 3: Question #1 Query

```sql
SELECT 
sum(new_player_count) AS new_players_sum,
sum(retained_count) AS new_players_retained,
(sum(retained_count)/sum(new_player_count)) AS retention
FROM 
(
SELECT
  day_joined,
  new_player_count,
  retained_count,
  retained_count/new_player_count AS retention_rate
FROM (
  SELECT
    joined AS day_joined,
    COUNT(retained_status) AS retained_count
  FROM (
    SELECT
      player_id,
      joined,
      CASE
        WHEN MAX(days_between) > 30 THEN "retained"
      ELSE
      "not-retained"
    END
      AS retained_status
    FROM (
      SELECT
        player_info.player_id,
        player_info.joined,
        matches_info.day AS day_played,
        matches_info.match_id,
        (matches_info.day - player_info.joined) AS days_between
      FROM
        `ninth-botany-329514.gamecompanydata.player_info` AS player_info
      JOIN
        `ninth-botany-329514.gamecompanydata.matches_info` AS matches_info
      ON
        player_info.player_id=matches_info.player_id
      ORDER BY
        joined,
        player_id,
        day_played)
    GROUP BY
      player_id,
      joined
    ORDER BY
      joined,
      player_id)
  WHERE
    retained_status = "retained"
  GROUP BY
    day_joined)
JOIN (
  SELECT
    joined,
    COUNT(DISTINCT player_id) AS new_player_count
  FROM
    `ninth-botany-329514.gamecompanydata.player_info`
  GROUP BY
    joined)
ON
  day_joined = joined
)
```

### Appendix 4: Question #3 Query

```sql
SELECT 
location,
COUNT(CASE WHEN status = 'retained' THEN 1 END) AS retained_count,
COUNT(CASE WHEN status = 'not-retained' THEN 1 END) AS notretained_count
FROM(
SELECT
  player_info.player_id AS player_id,
  player_info.location AS location,
  player_info.age AS age,
  retention_status.retention_status AS status
FROM
  `ninth-botany-329514.gamecompanydata.player_info` AS player_info
JOIN
`ninth-botany-329514.gamecompanydata.retention_status` AS retention_status
ON
player_info.player_id=retention_status.player_id
ORDER BY
location, age, status
)
GROUP BY location
```

### Appendix 5: Question #4 Query

```sql
SELECT
age_group,
count(*) AS players_agegroup
FROM(
SELECT
  age,
  CASE
    WHEN age<12 THEN 'under_12' 
    WHEN age<16 THEN '12_to_15' 
    WHEN age<20 THEN '16_to_19' 
    WHEN age<24 THEN '20_to_23' 
    WHEN age<28 THEN '24_to_27'
    WHEN age<32 THEN '28_to_31'
    ELSE '32_and_up' END AS age_group 
FROM
  `ninth-botany-329514.gamecompanydata.player_info`
  )
GROUP BY age_group
ORDER BY age_group
```

### Appendix 6: Question #5 Query

```sql
SELECT location, count(*) as player_count
FROM
  `jkjkjk-329514.Game_Company_Data2.player_info`
GROUP BY location
```

### Appendix 7: Question #6 Query

```sql
SELECT
location,
SUM(price) spend_by_region
FROM(
SELECT
purchase_info.player_id AS player_id,
item_info.item_id AS item_id,
item_info.price AS price,
player_info.location AS location
FROM
  `ninth-botany-329514.gamecompanydata.purchase_info` AS purchase_info
JOIN 
`ninth-botany-329514.gamecompanydata.item_info` AS item_info
ON 
purchase_info.item_id=item_info.item_id
JOIN 
`ninth-botany-329514.gamecompanydata.player_info` AS player_info
ON 
purchase_info.player_id=player_info.player_id
)
GROUP BY location
```


### Appendix 8: Question #7 Query

```sql
SELECT
  age,
  COUNT(*) AS users_this_age,
  NTILE(4) OVER (ORDER BY age) AS quartile
FROM
  `ninth-botany-329514.gamecompanydata.player_info`
GROUP BY
  age
ORDER BY
  age
  ```
