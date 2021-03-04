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



### Table

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



##### SQL注入攻击

Use `''` or `\'`



#### Date

`to_date( '07/20/1969' ,'MM/DD/YYYY)`

- Current time :
    - Global :
        - `CURRENT_DATE(no time)`
        - `CURRENT_TIMESTAMP(time included)`
    - Other functions owned by different languages themselves.

Convert the Date data by functions which ‘cast’ data types to avoid bad surprises.

```sql
select * from forum_posts where post_date >= '2018-03-12';				// do not
select * from forum_posts where post_date >= date('2018-03-12');
select * from forum_posts where post_date >= date('12 March, 2018');
```

##### Datetime

- Change Date to Datetime will set Time to `00:00:00`
- Solution:
    - Set Time to `23:59:59`

Date Arithmetic: `+1 month` != `+30 days` 



#### Select

To display the full content of a table. (Not recommended)

```sql
select * from tablename				// print table
```

Too many data may block the internet. Let the DB Server to handle the computation.



##### To select a Row

```sql
select * from movies		// movies is a column name
where country = 'us'
```

You can compare to a number, a string constant(use `‘’`, not `“”`), another column (from the same table or another, we'll see queries involving several tables later) or the result of a function (we'll see them soon)

嵌套：

Type1:

```sql
select *
from (select * from movies
     	where country = 'us') as us_movies
where year_released between 1940 and 1949
```

Type2:

```sql
select *
from (select * from movies 
     	where year_released between 1940 and 1949)
     movies_from_the_1040s
where country = 'us'
```

Type3: (simplest)

```sql
select *
from movies
where country = 'us'
	and year_released between 1940 and 1949
```

##### priority level

==`and` > `or`== : may cause a problem.

```sql
where country = 'us'
	or country = 'gb'
	and year_released between 1940 and 1949
```

First it select `country = ‘us’ and year_released between 1940 and 1949`, then `or country = 'gb’`, thus no British movies are selected.

```sql
where (country = 'us'
	or country = 'gb')
	and year_released between 1940 and 1949
```

`where (country = 'us' or country = 'gb')` == `where country in ('us', 'gb')`

##### Comparison operators

```sql
=					#equal
<>	or	!=			#not equal
<		<=
>		>=
```

Comparison:

- `2 < 10`
- `'2' < '10'` : wrong
- `'2-JUN-1883' > '1-DEC-2056'` : wrong
- String uses lexicographical order

##### like

Search for a pattern. 

- `%`: any number of characters, including none
- `_`: one and only one character

```sql
select * from movies
where title not like '%A%'
	and title not like '%a%'
```



#### NULL

In SQL, a NULL denotes that a value is *missing*. ==NULL is Not A Value!==

```sql
where column_name = null			# wrong!
where column_name <> null			# wrong!

where column_name is null
where column_name is not null
```

- Check for alive people

    ```sql
    select * from people where died is null
    ```

    ```sql
    select peopleid, surname,
    	date_part('year', now())-born as age
    from people
    where died is null
    ```

    



#### Link two Strings

```sql
'hello' || 'world'
'hello' + 'world'
concat('hello') 'world'
```

- Example:

    ```sql
    select title
    	|| 'was released in'
    	|| year_released
    	as movie_release
    from movies
    where country = 'us'
    ```

- Try:

    ```sql
    select 'hello'				# output is 'hello'
    select 2>1					# output is 'ture'
    ```





### Functions



```sql
select title, year_released
from movies
where country = 'us'
```

- Avoid leaking any internal messages, like id. 

Try to avoid using functions after `where`. After `select` is ok.

`round()`: 四舍五入

`trunc()`: 截断小数`trunc(3.141592, 3)` 3.141

`upper()`

`lower()`

`substr()`

```sql
substr('Citizen Kane', 5, 3)			# start from 1
```

`trim()`:	Delete space on left and white

`replace()`

`cast()`: 类型转换