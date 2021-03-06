[![Build Status](https://www.travis-ci.org/scull7/bs-sql-composer.svg?branch=master)](https://www.travis-ci.org/scull7/bs-sql-composer)
[![Coverage Status](https://coveralls.io/repos/github/scull7/bs-sql-composer/badge.svg?branch=master)](https://coveralls.io/github/scull7/bs-sql-composer?branch=master)

# bs-sql-composer
A library for SQL composition in BucklesScript

## Usage

### Basic Select
```ocaml
  let _ = SqlComposer.Select.(
    select
    |> field "*"
    |> from "test"
    |> to_sql
    |> Js.log
  )
```
```sql
SELECT
  *
FROM test
WHERE 1=1
```

### Where Clause
```ocaml
  let _ = SqlComposer.Select.(
    select
    |> field "*"
    |> from "test"
    |> where "AND test.foo = ?"
    |> to_sql
    |> Js.log
  )
```
```sql
SELECT
  *
FROM test
WHERE 1=1
AND test.foo = ?
```

### Join Clause
```ocaml
  let _ = SqlComposer.Select.(
    select
    |> field "*"
    |> from "test"
    |> join "JOIN foo ON test.foo_id = foo.id"
    |> to_sql
    |> Js.log
  )
```
```sql
SELECT
  *
FROM test
JOIN foo ON test.foo_id = foo.id
WHERE 1=1
```

### Adding to a base query
```ocaml
  let _ = SqlComposer.Select.(
    let base_query =
      from "test"
      |> select "foo"
      |> select "bar"
    in
    where base_query "AND test.foo = ?"
    |> to_sql
    |> Js.log
  )
```
```sql
SELECT
  foo
, bar
FROM test
WHERE 1=1
AND test.foo = ?
```

### Alias a field
```ocaml
let _ = SqlComposer.Select.(
  select
  |> from "test"
  |> field "foo AS bar"
  |> to_sql
  |> Js.log
)
```
```sql
SELECT
  foo AS bar
FROM test
WHERE 1=1
```

### Order By
```ocaml
let _ = SqlComposer.Select.(
  select
  |> from "test"
  |> field "*"
  |> order_by (`Asc "foo")
  |> order_by (`Desc "bar")
  |> to_sql
  |> Js.log
)
```
```sql
SELECT
  *
FROM test
WHERE 1=1
ORDER BY
  foo ASC
, bar DESC
```

### Group By
```ocaml
let _ = SqlComposer.Select.(
  select
  |> from "test"
  |> field "foo AS bar"
  |> group_by "foo"
  |> group_by "thing"
)
```
```sql
SELECT
  foo AS bar
FROM test
WHERE 1=1
GROUP BY
  foo
, thing
```

### Select Distinct
```ocaml
let _ SqlComposer.Select.(
  select
  |> field "foo"
  |> from "test"
  |> modifier `Distinct
)
```
```sql
SELECT DISTINCT
  foo
FROM test
WHERE 1=1
```

## How do I install it?

Inside of a BuckleScript project:

```shell
yarn install --save bs-sql-composer
```

Then add `bs-sql-composer` to your `bs-dependencies` in `bsconfig.json`:

```json
{
  "bs-dependencies": [ "bs-sql-composer" ]
}
```
