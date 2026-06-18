# HW15

This homework reviews JDBC, Hibernate, JPA, entity lifecycle, ORM concepts, and common SQL join and union questions.

Source page: [HW 15 on Notion](TO_BE_UPDATED)

## 1. JDBC vs Hibernate

JDBC is a lower-level Java API for talking to the database directly.

With JDBC, I usually:

- write SQL manually
- open connection
- create statement
- execute query
- map result set to Java objects
- close resources

Hibernate is an ORM framework.

With Hibernate, I usually:

- work with Java entities
- map classes to tables
- let Hibernate generate or manage much of the SQL
- use session and transaction APIs

My summary:

- JDBC gives more direct SQL control
- Hibernate gives higher productivity and less boilerplate
- JDBC is more manual
- Hibernate is more object-oriented

## 2. Statement vs PreparedStatement vs CallableStatement

### Statement

`Statement` is the simplest way to execute SQL, but it usually builds SQL as a plain string.

Example use:

- simple static SQL

Problems:

- not safe for dynamic user input
- easier to cause SQL injection
- less efficient for repeated execution

### PreparedStatement

`PreparedStatement` uses placeholders like `?` and binds parameters separately.

Benefits:

- helps prevent SQL injection
- better for dynamic input
- can be precompiled and reused
- usually the preferred option for normal CRUD operations

### CallableStatement

`CallableStatement` is used to call stored procedures in the database.

Use case:

- call stored procedure with input and output parameters

## 3. How to prevent SQL injection

Main ways:

1. Use `PreparedStatement` instead of building SQL by string concatenation
2. Validate and sanitize input
3. Use least-privilege database accounts
4. Avoid exposing raw SQL construction to user input
5. Use ORM or repository patterns correctly so parameters are bound safely

Bad example:

```java
String sql = "SELECT * FROM users WHERE name = '" + name + "'";
```

Better example:

```java
String sql = "SELECT * FROM users WHERE name = ?";
PreparedStatement ps = connection.prepareStatement(sql);
ps.setString(1, name);
```

## 4. What is ORM

ORM means Object-Relational Mapping.

It is a way to map Java objects to relational database tables.

For example:

- Java class -> table
- field -> column
- object relationship -> table relationship

Benefits:

- less SQL boilerplate
- easier CRUD
- object-oriented programming style

Tradeoff:

- developers still need to understand generated SQL and performance behavior

## 5. JPA vs Hibernate

JPA is a specification.

Hibernate is an implementation of ORM and also one of the most common JPA providers.

My summary:

- JPA defines the standard API and annotations
- Hibernate is the actual framework that implements that behavior
- If I use Spring Data JPA with Hibernate, then JPA is the standard interface and Hibernate is the engine underneath

## 6. What are the persistent states in Entity LifeCycle

Common entity lifecycle states are:

### Transient

The object is created in Java memory, but it is not managed by the persistence context and not saved in the database yet.

### Persistent / Managed

The entity is being managed by the persistence context. Changes to it can be tracked and synchronized to the database.

### Detached

The entity was previously managed, but now it is no longer attached to the current persistence context.

### Removed

The entity is marked for deletion and will be deleted from the database when the transaction is committed.

## 7. Mapping relationship

Common ORM relationships are:

- one-to-one
- one-to-many
- many-to-one
- many-to-many

Examples:

- one department has many employees -> one-to-many
- many employees belong to one department -> many-to-one

In JPA or Hibernate, these are usually mapped with annotations such as:

- `@OneToOne`
- `@OneToMany`
- `@ManyToOne`
- `@ManyToMany`

## 8. What is the cascade type

Cascade type controls whether an operation on a parent entity should also be applied to related child entities.

Common cascade types:

- `PERSIST`
- `MERGE`
- `REMOVE`
- `REFRESH`
- `DETACH`
- `ALL`

Example:

- if a parent entity is saved and cascade persist is enabled, child entities can also be saved automatically

## 9. What is the fetch type

Fetch type controls when related entities are loaded.

Two common types:

- `EAGER`
- `LAZY`

### EAGER

Load related data immediately.

### LAZY

Load related data only when it is actually accessed.

My summary:

- `EAGER` is simpler but can load too much data
- `LAZY` is usually better for performance, but developers need to understand session scope and lazy loading behavior

## 10. What is the first-level cache and second-level cache

### First-level cache

The first-level cache is the session-level cache.

It exists by default in Hibernate.

That means if the same entity is loaded twice inside the same session, Hibernate can reuse it from the session cache instead of querying the database again.

### Second-level cache

The second-level cache is shared across sessions.

It is optional and must be configured separately.

It can improve performance when the same data is read often across multiple sessions or requests.

My summary:

- first-level cache = session scope, default
- second-level cache = shared cache, optional

## 11. Left join vs right join vs inner join vs outer join vs cross join

### Inner join

Returns only matching rows from both tables.

### Left join

Returns all rows from the left table, plus matching rows from the right table. If there is no match, the right side becomes `NULL`.

### Right join

Returns all rows from the right table, plus matching rows from the left table. If there is no match, the left side becomes `NULL`.

### Outer join

Usually people mean full outer join.

It returns all rows from both tables, whether matched or not. Unmatched side becomes `NULL`.

### Cross join

Returns the Cartesian product of two tables.

If table A has 3 rows and table B has 4 rows, cross join returns 12 rows.

My summary:

- inner = only matches
- left = all left + matches
- right = all right + matches
- full outer = all both sides
- cross = every combination

## 12. Union vs union all

### UNION

Combines result sets and removes duplicates.

### UNION ALL

Combines result sets and keeps duplicates.

My summary:

- `UNION` does deduplication, so it may cost more
- `UNION ALL` is usually faster if duplicates are acceptable

## Final summary

- JDBC is lower level and SQL-driven, while Hibernate is higher level and ORM-driven.
- `PreparedStatement` is the usual safe choice for dynamic SQL and helps prevent SQL injection.
- ORM maps Java objects to relational tables.
- JPA is the specification, and Hibernate is a common implementation.
- Entity lifecycle includes transient, persistent, detached, and removed states.
- Cascade type controls related entity operations, and fetch type controls loading timing.
- First-level cache is session-level, and second-level cache is shared and optional.
- Joins control how tables are combined, and `UNION` / `UNION ALL` control how result sets are merged.
