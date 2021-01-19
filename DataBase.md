# DataBase

[toc]







## PostgreSQL

- Command Line :

    `psql postgres`

- To start service manually :

    `pg_ctl -D /usr/local/var/postgres start`	  # To start

    `pg_ctl -D /usr/local/var/postgres stop`		# To stop

- Automatically :

    `brew service start postgresql`    # To start

    `brew service stop postgresql`      # To stop







## SQL

Many kinds of SQL Languages, like PostgreSQL, ORACLE, MySQL, SQL Server...



- don’t use `delete`, need to identify what to delete, otherwise will easily case chaos.



#### Create a Table

- table_name = TABLENAME = TaBleNamE (same with the Identifiers, columns)

```sql
create table	table_name (
	column_name 	datatype,
    ...				...		,
    ...
	)
```



#### Datatypes

##### Text datatypes

`char(length)` : string with certain length.

`varchar(max length)` & `varchar2(max length)`~ORACLE~ : string with variable length.

`clob` : (Character Large Object) very long string (like a novel).

##### Number datatypes

`int` 

`float`

`numeric (precision, scale)` : like numeric(8,2) can store at most `99999999.99`

##### Date datatypes

`date` : includes time, down to second with Oracle, not with other products

`datetime` : down to second (other than Orale, DB2 and PostreSQL)

`timestamp` : down to 0.000001 second, Replace dateline in DB2/PostgreSQL

##### Binary datatypes

`raw(max length)` : fixed length

`varbinary(max length)` : with a variable length

`blob` & `bytea`~PostgreSQL~ : (Binary Large Object) very long binary data, can be used to store pictures.

##### An Example

```sql
create table people (	peopleid	int,
                    	first_name	varchar(30),
                     	surname		varchar(30),
                     	born		numeric(4),
                     	died		numeric(4)
                    )
```



#### How to prevent repetition

- Primary Key
    1. Unique
    2. Mandatory

- `not null`

- `check (surname = upper(surname))` : before the comma

- `unique (first_name, surname)` : after the comma

```sql
create table people (pelpleid 		int not null primary key,
                     first_name 	varchar(30)
                     	check (first_name = upper(first_name)),
                     surname 		varchar(30) not null
                     	check (surname = upper(surname)),
                     born 			numeric(4) not null,
                     died 			numeric(4),
                     	unique (first_name, surname)
                    )
```



- `foreign key` : ID from another table. ( always check their existance! )

```sql
create table movies (movieid 		int not null primary key,
                     title 			varchar(60) not null,
                     country 		char(2) not null,
                     year_released 	numeric(4) not null
                     	check(year_released >= 1895),
                     unique (title, country, year_released)
                     		foreign key(country)
                     			references countries(country_code))
```

If they do not exist, return error.





#### Insert

```sql
insert into table_name (list of columns)
values (list of values)
```

- Example:

```sql
insert into countries(country_code, country_name, continent)
values('us', 'United States', 'AMERICA')
```

- `(country_code, country_name, continent)` can be omitted, but please don’t do so.



#### SQL注入攻击

Use `''` or `\'`



#### Date

`to_date( '07/20/1969' ,'MM/DD/YYYY)`

- Current time :
    - Global :
        - `CURRENT_DATE(no time)`
        - `CURRENT_TIMESTAMP(time included)`
    - Other functions owned by different languages themselves.

