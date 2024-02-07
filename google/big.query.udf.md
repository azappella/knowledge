# big query - User-defined functions

A user-defined function (UDF) lets you create a function by using a SQL expression or JavaScript code. A UDF accepts columns of input, performs actions on the input, and returns the result of those actions as a value.

You can define UDFs as either persistent or temporary. You can reuse persistent UDFs across multiple queries, while temporary UDFs only exist in the scope of a single query.

To create a UDF, use the CREATE FUNCTION statement. To delete a persistent user-defined function, use the DROP FUNCTION statement. Temporary UDFs expire as soon as the query finishes. The DROP FUNCTION statement is only supported for temporary UDFs in multi-statement queries and procedures.

For information on UDFs in legacy SQL, see User-defined functions in legacy SQL.
SQL UDFs

The following example creates a temporary SQL UDF named AddFourAndDivide and calls the UDF from within a SELECT statement:

```
CREATE TEMP FUNCTION AddFourAndDivide(x INT64, y INT64)
RETURNS FLOAT64
AS (
  (x + 4) / y
);SELECT
  val, AddFourAndDivide(val, 2)
FROM
  UNNEST([2,3,5,8]) AS val;

```
This example produces the following output:

+-----+-----+
| val | f0_ |
+-----+-----+
|   2 | 3.0 |
|   3 | 3.5 |
|   5 | 4.5 |
|   8 | 6.0 |
+-----+-----+

The next example creates the same function as a persistent UDF:
```
CREATE FUNCTION mydataset.AddFourAndDivide(x INT64, y INT64)
RETURNS FLOAT64
AS (
  (x + 4) / y
);
```
Because this UDF is persistent, you must specify a dataset for the function (mydataset in this example). After you run the CREATE FUNCTION statement, you can call the function from a query:
```
SELECT
  val, mydataset.AddFourAndDivide(val, 2)
FROM
  UNNEST([2,3,5,8,12]) AS val;
```
Templated SQL UDF parameters

A parameter with a type equal to ANY TYPE can match more than one argument type when the function is called.

    If more than one parameter has type ANY TYPE, then BigQuery doesn't enforce any type relationship between these arguments.
    The function return type cannot be ANY TYPE. It must be either omitted, which means to be automatically determined based on sql_expression, or an explicit type.
    Passing the function arguments of types that are incompatible with the function definition results in an error at call time.

The following example shows a SQL UDF that uses a templated parameter.

```
CREATE TEMP FUNCTION addFourAndDivideAny(x ANY TYPE, y ANY TYPE)
AS (
  (x + 4) / y
);SELECT
  addFourAndDivideAny(3, 4) AS integer_input,
  addFourAndDivideAny(1.59, 3.14) AS floating_point_input;
```
This example produces the following output:

+----------------+-----------------------+
| integer_input  |  floating_point_input |
+----------------+-----------------------+
| 1.75           | 1.7802547770700636    |
+----------------+-----------------------+

The next example uses a templated parameter to return the last element of an array of any type:
```
CREATE TEMP FUNCTION lastArrayElement(arr ANY TYPE)
AS (
  arr[ORDINAL(ARRAY_LENGTH(arr))]
);SELECT
  lastArrayElement(x) AS last_element
FROM (
  SELECT [2,3,5,8,13] AS x
);
```

This example produces the following output:

+--------------+
| last_element |
+--------------+
| 13           |
+--------------+

Scalar subqueries

A SQL UDF can return the value of a scalar subquery. A scalar subquery must select a single column.

The following example shows a SQL UDF that uses a scalar subquery to count the number of users with a given age in a user table:

```
CREATE TEMP TABLE users
AS (
  SELECT
    1 AS id, 10 AS age
  UNION ALL
  SELECT
    2 AS id, 30 AS age
  UNION ALL
  SELECT
    3 AS id, 10 AS age
);CREATE TEMP FUNCTION countUserByAge(userAge INT64)
AS (
  (SELECT COUNT(1) FROM users WHERE age = userAge)
);SELECT
  countUserByAge(10) AS count_user_age_10,
  countUserByAge(20) AS count_user_age_20,
  countUserByAge(30) AS count_user_age_30;
```
This example produces the following output:

+-------------------+-------------------+-------------------+
| count_user_age_10 | count_user_age_20 | count_user_age_30 |
+-------------------+-------------------+-------------------+
|                 2 |                 0 |                 1 |
+-------------------+-------------------+-------------------+

Default project in SQL expressions

In the body of a SQL UDF, any references to BigQuery entities, such as tables or views, must include the project ID, unless the entity resides in the same project that runs the CREATE FUNCTION statement.

For example, consider the following statement:
```
CREATE FUNCTION project1.mydataset.myfunction()
AS (
  (SELECT COUNT(*) FROM mydataset.mytable)
);
```
If you run this statement from project1 and mydataset.mytable exists in project1, then the statement succeeds. However, if you run this statement from a different project, then the statement fails. To correct the error, include the project ID in the table reference:
```
CREATE FUNCTION project1.mydataset.myfunction()
AS (
  (SELECT COUNT(*) FROM project1.mydataset.mytable)
);
```
You can also reference an entity in a different project or dataset from the one where you create the function:
```
CREATE FUNCTION project1.mydataset.myfunction()
AS (
  (SELECT COUNT(*) FROM project2.another_dataset.another_table)
);
```

## JavaScript UDFs

A JavaScript UDF lets you call code written in JavaScript from a SQL query. JavaScript UDFs typically consume more slot resources as compared to standard SQL queries, decreasing job performance. If the function can be expressed in SQL, it is often more optimal to run the code as a standard SQL query job.

The following example shows a JavaScript UDF. The JavaScript code is quoted within a raw string.

```
CREATE TEMP FUNCTION multiplyInputs(x FLOAT64, y FLOAT64)
RETURNS FLOAT64
LANGUAGE js
AS r"""
  return x*y;
""";WITH numbers AS
  (SELECT 1 AS x, 5 as y
  UNION ALL
  SELECT 2 AS x, 10 as y
  UNION ALL
  SELECT 3 as x, 15 as y)
SELECT x, y, multiplyInputs(x, y) AS product
FROM numbers;
```
This example produces the following output:

+-----+-----+--------------+
| x   | y   | product      |
+-----+-----+--------------+
| 1   | 5   | 5            |
| 2   | 10  | 20           |
| 3   | 15  | 45           |
+-----+-----+--------------+

The next example sums the values of all fields named foo in the given JSON string.
```
CREATE TEMP FUNCTION SumFieldsNamedFoo(json_row STRING)
RETURNS FLOAT64
LANGUAGE js
AS r"""
  function SumFoo(obj) {
    var sum = 0;
    for (var field in obj) {
      if (obj.hasOwnProperty(field) && obj[field] != null) {
        if (typeof obj[field] == "object") {
          sum += SumFoo(obj[field]);
        } else if (field == "foo") {
          sum += obj[field];
        }
      }
    }
    return sum;
  }
  var row = JSON.parse(json_row);
  return SumFoo(row);
""";WITH Input AS (
  SELECT
    STRUCT(1 AS foo, 2 AS bar, STRUCT('foo' AS x, 3.14 AS foo) AS baz) AS s,
    10 AS foo
  UNION ALL
  SELECT
    NULL,
    4 AS foo
  UNION ALL
  SELECT
    STRUCT(NULL, 2 AS bar, STRUCT('fizz' AS x, 1.59 AS foo) AS baz) AS s,
    NULL AS foo
)
SELECT
  TO_JSON_STRING(t) AS json_row,
  SumFieldsNamedFoo(TO_JSON_STRING(t)) AS foo_sum
FROM Input AS t;
```
The example produces the following output:

+---------------------------------------------------------------------+---------+
| json_row                                                            | foo_sum |
+---------------------------------------------------------------------+---------+
| {"s":{"foo":1,"bar":2,"baz":{"x":"foo","foo":3.14}},"foo":10}       | 14.14   |
| {"s":null,"foo":4}                                                  | 4       |
| {"s":{"foo":null,"bar":2,"baz":{"x":"fizz","foo":1.59}},"foo":null} | 1.59    |
+---------------------------------------------------------------------+---------+

### Supported JavaScript UDF data types

Some SQL types have a direct mapping to JavaScript types, but others don't. BigQuery represents types in the following manner:

BigQuery data type 	JavaScript data type
ARRAY 	ARRAY
BOOL 	BOOLEAN
BYTES 	base64-encoded STRING
FLOAT64 	NUMBER
NUMERIC, BIGNUMERIC 	If a NUMERIC or BIGNUMERIC value can be represented exactly as an IEEE 754 floating-point value and has no fractional part, the value is encoded as a Number. These values are in the range [-253, 253]. Otherwise, the value is encoded as a string.
STRING 	STRING
STRUCT 	OBJECT where each STRUCT field is a named field
TIMESTAMP 	DATE with a microsecond field containing the microsecond fraction of the timestamp
DATE 	DATE
JSON 	

JSON OBJECTS, ARRAYS, and VALUES are converted into equivalent JavaScript OBJECTS, ARRAYS, and VALUES.

JavaScript does not support INT64 values. Only JSON numbers in the range [-253, 253] are converted exactly. Otherwise, the numeric value is rounded, which could result in a loss of precision.

Because JavaScript does not support a 64-bit integer type, INT64 is unsupported as an input type for JavaScript UDFs. Instead, use FLOAT64 to represent integer values as a number, or STRING to represent integer values as a string.

BigQuery does support INT64 as a return type in JavaScript UDFs. In this case, the JavaScript function body can return either a JavaScript Number or a String. BigQuery then converts either of these types to INT64.

If the return value of the JavaScript UDF is a Promise, BigQuery waits for the Promise until Promise is settled. If the Promise settles into a fulfilled state, BigQuery returns its result. If the Promise settles into a rejected state, BigQuery returns an error.
Quote rules

You must enclose JavaScript code in quotes. For simple, one line code snippets, you can use a standard quoted string:
```
CREATE TEMP FUNCTION plusOne(x FLOAT64)
RETURNS FLOAT64
LANGUAGE js
AS "return x+1;";SELECT val, plusOne(val) AS result
FROM UNNEST([1, 2, 3, 4, 5]) AS val;
```
This example produces the following output:

+-----------+-----------+
| val       | result    |
+-----------+-----------+
| 1         | 2.0       |
| 2         | 3.0       |
| 3         | 4.0       |
| 4         | 5.0       |
| 5         | 6.0       |
+-----------+-----------+

In cases where the snippet contains quotes, or consists of multiple lines, use triple-quoted blocks:
```
CREATE TEMP FUNCTION customGreeting(a STRING)
RETURNS STRING
LANGUAGE js
AS r"""
  var d = new Date();
  if (d.getHours() < 12) {
    return 'Good Morning, ' + a + '!';
  } else {
    return 'Good Evening, ' + a + '!';
  }
""";SELECT customGreeting(names) AS everyone
FROM UNNEST(['Hannah', 'Max', 'Jakob']) AS names;
```
This example produces the following output:

+-----------------------+
| everyone              |
+-----------------------+
| Good Morning, Hannah! |
| Good Morning, Max!    |
| Good Morning, Jakob!  |
+-----------------------+

Include JavaScript libraries

You can extend your JavaScript UDFs using the OPTIONS section. This section lets you specify external code libraries for the UDF.
```
CREATE TEMP FUNCTION myFunc(a FLOAT64, b STRING)
RETURNS STRING
LANGUAGE js
  OPTIONS (
    library=['gs://my-bucket/path/to/lib1.js', 'gs://my-bucket/path/to/lib2.js'])
AS r"""
  // Assumes 'doInterestingStuff' is defined in one of the library files.
  return doInterestingStuff(a, b);
""";SELECT myFunc(3.14, 'foo');
```
In the preceding example, code in lib1.js and lib2.js is available to any code in the [external_code] section of the UDF.
Best practices for JavaScript UDFs

Prefilter your input

If your input can be filtered down before being passed to a JavaScript UDF, your query might be faster and cheaper.

Avoid persistent mutable state

Do not store or access mutable state across JavaScript UDF calls. For example, avoid the following pattern:
```
-- Avoid this pattern
CREATE FUNCTION temp.mutable()
RETURNS INT64
LANGUAGE js
AS r"""
  var i = 0; // Mutable state
  function dontDoThis() {
    return ++i;
  }
  return dontDoThis()
""";
```
Use memory efficiently

The JavaScript processing environment has limited memory available per query. JavaScript UDF queries that accumulate too much local state might fail due to memory exhaustion.

You can authorize UDFs as routines. Authorized routines let you share query results with specific users or groups without giving them access to the underlying tables that generated the results. For example, an authorized routine can compute an aggregation over data or look up a table value and use that value in a computation. For more information, see Authorized routines.
Add descriptions to UDFs

To add a description to a UDF, follow these steps:

    Go to the BigQuery page in the Google Cloud console.

    Go to BigQuery

    In the Explorer panel, expand your project and dataset, then select the function.

    In the Details pane, click Edit Routine Details to edit the description text.

    In the dialog, enter a description in the box or edit the existing description. Click Save to save the new description text.

To update the description of a function, recreate your function using the CREATE FUNCTION DDL statement and set the description field in the OPTIONS list:

    In the Google Cloud console, go to the BigQuery page.

    Go to BigQuery

    In the query editor, enter the following statement:

    CREATE OR REPLACE FUNCTION mydataset.my_function(...)
    AS (
      ...
    ) OPTIONS (
      description = 'DESCRIPTION'
    );

    Click Run.

For more information about how to run queries, see Running interactive queries.
Create custom masking routines

You can create UDFs for use with custom masking routines. Custom masking routines must meet the following requirements:

    The custom masking routine must be a SQL UDF.
    In the function OPTIONS, the data_governance_type option must be set to DATA_MASKING.
    Custom masking routines support the following functions:
        REGEXP_REPLACE string function
        FARM_FINGERPINT hash function
        MD5 hash function
        SHA1 hash function
        SHA256 hash function
        SHA512 hash function
        CAST conversion function
        CONCAT string function
    Custom masking routines can accept either no inputs or one input within BigQuery data types, with the exception of GEOGRAPHY and STRUCT. GEOGRAPHY and STRUCT are not supported for custom masking routines.
    Templated SQL UDF parameters are not supported.
    When an input is provided, the input and output data types must be the same.
    An output type must be provided.
    No other UDFs, subqueries, tables, or views can be referenced in the definition body.
    After creating a masking routine, the routine cannot be changed to a standard function. This means that if the data_governance_type option is set to DATA_MASKING, then you cannot change data_governance_type using DDL statements or API calls.

For example, a masking routine that replaces a user's social security number with XXX-XX-XXXX might look as follows:
```
  CREATE OR REPLACE FUNCTION 
SSN_Mask
(ssn STRING) RETURNS STRING
  OPTIONS (data_governance_type="DATA_MASKING") AS (
  SAFE.REGEXP_REPLACE(ssn, '[0-9]', 'X') # 123-45-6789 -> XXX-XX-XXXX
  );
```
The following example hashes with user provided salt, using the SHA256 function:
```
CREATE OR REPLACE FUNCTION `project.dataset.masking_routine1`(
  ssn STRING)
RETURNS STRING OPTIONS (data_governance_type = 'DATA_MASKING')
AS (
  CAST(SHA256(CONCAT(ssn, 'salt')) AS STRING format 'HEX')
);
```
The following example masks a DATETIME column with a constant value:
```
CREATE OR REPLACE FUNCTION `project.dataset.masking_routine2`(
  column DATETIME)
RETURNS DATETIME OPTIONS (data_governance_type = 'DATA_MASKING')
AS (
  SAFE_CAST('2023-09-07' AS DATETIME)
);
```
After you create the custom masking routine, it's available as a masking rule in Create data policies.

Community contributed UDFs are available in the bigquery-public-data.persistent_udfs public dataset and the open source bigquery-utils GitHub repository. You can see all the community UDFs in the Google Cloud console by starring the bigquery-public-data project in the Explorer pane, and then expanding the nested persistent_udfs dataset within that project.

If you want to contribute to the UDFs in this repository, see Contributing UDFs for instructions.
Limitations

The following limitations apply to temporary and persistent user-defined functions:

    The DOM objects Window, Document, and Node, and functions that require them, are not supported.
    JavaScript functions that rely on native code can fail, for example, if they make restricted system calls.
    A JavaScript UDF can time out and prevent your query from completing. Timeouts can be as short as 5 minutes, but can vary depending on several factors, including how much user CPU time your function consumes and how large your inputs and outputs to the JavaScript function are.
    Bitwise operations in JavaScript handle only the most significant 32 bits.
    UDFs are subject to certain rate limits and quota limits. For more information, see UDF limits.

The following limitations apply to persistent user-defined functions:

    Each dataset can only contain one persistent UDF with the same name. However, you can create a UDF whose name is the same as the name of a table in the same dataset.
    When referencing a persistent UDF from another persistent UDF or a logical view, you must qualify the name with the dataset. For example:
    CREATE FUNCTION mydataset.referringFunction() AS (mydataset.referencedFunction());

The following limitations apply to temporary user-defined functions.

    When creating a temporary UDF, function_name cannot contain periods.
    Views and persistent UDFs cannot reference temporary UDFs.


## Reference

- https://cloud.google.com/bigquery/docs/user-defined-functions

