# NPPES NPI + Postgres

Quickly load National Provider Identifier (NPI) and Health Care Provider Taxonomy data into Postgres

## NPI

### Download Data

* [Full Replacement Monthly NPI CSV](http://download.cms.gov/nppes/NPI_Files.html) ~ 600MB
* [Health Care Provider Taxonomy Code Set CSV](http://www.nucc.org/index.php?option=com_content&view=article&id=107&Itemid=132)

### Clean Data

```sh
# Replace empty "" integer fields in NPI CSV file
$ sed 's/""//g' npidata_20050523-20160110.csv > npi.csv

# Convert taxonomy data to utf-8 and delimiter to "|"
$ iconv -c -t utf8 nucc_taxonomy_160.csv | csvformat -D "|" > taxonomy.pipe
```

### Create `npi` Database

Make sure to change the paths to datasets in `create_npi_db.psql`

```sh
$ createdb -O 'username' npi
$ psql -U username -d npi -a -f create_npi_db.psql
```
