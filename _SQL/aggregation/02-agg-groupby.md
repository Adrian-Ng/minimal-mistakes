---
title: "Summarizing Data: Group By"
permalink: /SQL/aggregation/groupby/
excerpt: "Aggregating with the group by clause by Adrian Ng"
toc: false
header:
  overlay_color: "#000"
  overlay_filter: "0.5"
  overlay_image: /assets/images/headers/aggregation.jpg
---

## Intro

Let's get a distinct list of countries in our database. We can do this in two ways:

### Example 1

```sql
SELECT DISTINCT COUNTRY FROM music.Users;
```

### Example 2
```sql
SELECT
	COUNTRY
FROM music.Users
GROUP BY
	COUNTRY;
```

Both these examples return the same result set. So why use one over the other?
* Example 1 is less verbose. 
* Example 2 can be used with aggregation functions


