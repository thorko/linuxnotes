mysql performance debugging
========================

enable performance_schema
ist nur über Neustart möglich

```
[mysqld]
performance_schema=on
```

```
use performance_schema;
select digest_text, max_timer_wait from events_statements_summary_by_digest order by max_timer_wait desc limit 10;

SELECT digest_text
     , count_star
     , avg_timer_wait 
  FROM events_statements_summary_by_digest 
 ORDER BY avg_timer_wait DESC;

repair table <table>
optimize table <table>
```

