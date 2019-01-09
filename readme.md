# Loading data

The bulk-files comprise a dataset for loading into Elasticsearch via the bulk-API.

The txt-file is intended for MySQL.

## Loading data into MySQL
- Create table (storageunits.sql)
MySQL can be picky about loading null-values into the one integer-column in the table (`individualCount`), so after creating the table:
- Temporarily convert the column: `alter table storageunits modify individualCount varchar(32);`
- Load the data: `load data infile '/var/lib/mysql-files/storageunits.txt' into table storageunits FIELDS TERMINATED BY '|' LINES TERMINATED BY "\n";`
- Remove empty columns: `update storageunits set individualCount = -123 where individualCount = '';`
- Re-convert the column: `alter table storageunits modify individualCount int(6) default 0;`
- Set the null-values: `update storageunits set individualCount = null where individualCount = -123;`

