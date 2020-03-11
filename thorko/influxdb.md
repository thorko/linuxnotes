# influxdb
## api usage
```bash
curl -G 'http://localhost:8086/query?db=sensu' -u 'admin:password' --data-urlencode 'q=show measurements'
```

## backup/restore

```bash
influxd backup -portable <path-to-backup>
influxd restore -portable path-to-backup
```

## Retention
```ini
max-series-per-database = 100000000
# The maximum number of series allowed per database before writes are dropped
[retention]
enabled = true
check-interval = "30m0s"
```

create retention policy for database

```sql
CREATE RETENTION POLICY "two_hours" ON "food_data" DURATION 2h REPLICATION 1 DEFAULT
```

# cli

```bash
influx
> show databases
> use sensu
> show measurements
> show retention policies
> ALTER RETENTION POLICY <retention_policy_name> ON <database_name> DURATION <duration> REPLICATION <n> SHARD DURATION <duration> DEFAULT
> CREATE RETENTION POLICY <retention_policy_name> ON <database_name> DURATION <duration> REPLICATION <n> [SHARD DURATION <duration>] [DEFAULT]
> create retention policy "2month" on "rzneo" duration 1608h0m replication 0 default
```

