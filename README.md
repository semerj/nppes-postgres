# NPPES NPI + Postgres

Quickly load National Provider Identifier (NPI) CSV data into Postgres

## NPI

### Download Latest NPI Data

[Full Replacement Monthly NPI File](http://download.cms.gov/nppes/NPI_Files.html) ~ 600MB

### Clean NPI CSV File

Replace empty `""` integer fields in order to load into Postgres

```sh
$ sed 's/""//g' npidata_20050523-20160110.csv > npi.csv
```

### Create `npi` Database

```sh
$ createdb -O 'username' npi
$ psql -U username -d npi -a -f create_npi_db.psql
```

### Load NPI Data

```sql
COPY npi
FROM '/full/path/to/npi.csv'
WITH CSV HEADER
DELIMITER AS ','
NULL AS '';
```

## Taxonomy

### Download Provider Taxonomy Data

[Health Care Provider Taxonomy Code Set CSV](http://www.nucc.org/index.php?option=com_content&view=article&id=107&Itemid=132)

### Create `taxonomy` Table in `npi` Database

```sh
$ psql -U username -d npi -a -f create_taxonomy_db.psql
```

### Clean Taxonomy Data

Convert data to utf-8 and delimiter to `|` to avoid confusion for Postgres

```sh
$ iconv -c -t utf8 nucc_taxonomy_160.csv | csvformat -D "|" > taxonomy.tab
```

### Load Taxonomy Data

```sql
COPY taxonomy
FROM '/full/path/to/taxonomy.tab'
WITH CSV HEADER
DELIMITER AS ','
NULL AS '';
```