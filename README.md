# NPPES NPI + Postgres

Quickly load National Provider Identifier (NPI) CSV data into Postgres

### Download Latest Data

[Full Replacement Monthly NPI File](http://download.cms.gov/nppes/NPI_Files.html) ~ 600MB

### Clean NPI CSV File

Replace empty `""` integer fields in order to load into Postgres

```sh
$ sed 's/""//g' npidata_20050523-20160110.csv > clean_npidata_20050523-20160110.csv
```

### Create `npi` Database

```sh
$ psql -U username -d npi -a -f create_db.psql
```

### Load Data

```sql
COPY npi
FROM '/full/path/to/data/clean_npidata_20050523-20160110.csv'
WITH CSV HEADER
DELIMITER AS ','
NULL AS '';
```
