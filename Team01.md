## Team_01
### №1
```sql
WITH tmp_max_update AS (
	SELECT * FROM (SELECT crr.id, crr.name, crr.updated, crr.rate_to_usd AS last_rate_to_usd,
				   ROW_NUMBER() OVER(partition by crr.id order by crr.updated DESC) AS rn FROM currency crr) crrcy
	WHERE rn < 2
)

SELECT COALESCE("user".name, 'not defined') AS name,
	COALESCE("user".lastname, 'not defined') AS lastname,
	bl.type,
	SUM(bl.money) AS volume,
	COALESCE(tmu.name, 'not defined') AS currency_name,
	COALESCE(tmu.last_rate_to_usd, 1) AS last_rate_to_usd,
	(SUM(bl.money) * COALESCE(tmu.last_rate_to_usd, 1)) AS total_volume_in_usd
FROM balance bl
FULL JOIN "user" ON "user".id = bl.user_id
FULL JOIN tmp_max_update tmu ON tmu.id = bl.currency_id
GROUP BY 1, 2, 3, 5, 6, tmu.last_rate_to_usd
ORDER BY 1 DESC, 2 ASC, 3 ASC
```
![image](https://github.com/IAmIngibitor/DB-practice-in-college/assets/109351663/4e377485-98a1-4e26-897f-f4e8215a6e6d)  
### №2
```sql

```
