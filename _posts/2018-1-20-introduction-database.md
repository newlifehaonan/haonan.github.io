---
layout: post
title: "Introduction to Oracle Database"
date: 2018-1-6 11:30:20
tag:
- database
description: relational model, schema, sql/plsql, transaction, architecture
comments: true
---

# Introduction to Oracle Database

## Appendix

* [Overview of database system](#Overview)

* [Schema](#Schema)

* [Data Access](#Access)

* [Transaction Management](#transaction)

* [Architecture](#Architecture)

<hr />

## <a name = "Overview">Overview of Database System<a />

<hr />
### Relational Database

1. A `relation` is a set of `tuples`. A tuple is an unordered set of attribute values.

2. A relational database is a database that stores data in relations (tables).A `table` is a two-dimensional representation of a relation in the form of rows (tuples) and columns (attributes)

3. A relational database is a database that conforms to the relational model. The relational model has the following major aspects:

  * Structure

  * Operations

  * integrity rules

### Database Management System (DBMS)

A `database management system` (DBMS) is software that controls the storage, organization, and retrieval of data

### Relational Database Management System (RDBMS)

DBMS based on relational model

<hr />

## <a name = "Schema">Schema<a />

<hr />

## Schema and Schema Object
1. A database `schema` is a logical container for data structures, called schema objects.

2. Schema Object
  * `Table`:A table is a set of rows. A column identifies an attribute of the entity described by the table, whereas a row identifies an instance of the entity.

  * `Indexes` :Indexes are schema objects that contains an entry for each indexed row of the table or table cluster and provide direct, fast access to rows. Oracle Database supports several types of index. An index-organized table is a table in which the data is stored in an index structure.

  * `Partitions`:Partitions are pieces of large tables and indexes. Each partition has its own name and may optionally have its own storage characteristics.

  * `Views`:Views are customized presentations of data in one or more tables or other views. **You can think of them as stored queries.** Views do not actually contain data.

  * `Sequences`:A sequence is a user-created object that can be shared by multiple users to generate integers. **Typically, sequences are used to generate primary key values**.

  * `Dimensions`:A dimension defines a parent-child relationship between pairs of column sets, here all the columns of a column set must come from the same table. Dimensions are commonly used to categorize data such as customers, products, and time.

  * `Synonyms`:A synonym **is an alias** for another schema object. Because a synonym is simply an alias, **it requires no storage other than its definition in the data dictionary**.

  * `PL/SQL subprograms` and packages:PL/SQL is the Oracle procedural extension of SQL. A PL/SQL subprogram is a named PL/SQL block that can be invoked with a set of parameters. A PL/SQL package groups logically related PL/SQL types, variables, and subprograms. See "PL/SQL Subprograms" and "PL/SQL Packages".

<hr />

3. Schema Dependency

  * If the definition of object A references object B, then A is a `dependent object` with respect to B and B is a `referenced object` with respect to A.

  * A dependent object is always up to date with respect to its referenced objects. When a dependent object is created, the database tracks dependencies between the dependent object and its referenced objects. When a referenced object changes in a way that might affect a dependent object, the dependent object is marked invalid.

## <a name = "Access">Data Access<a />
<hr />

### Structured Query Language (SQL)

1. In contrast to procedural languages such as C, which describe how things should be done, SQL is nonprocedural and describes what should be done

2. SQL statements enable you to perform the following tasks:

  * Query data

  * Insert, update, and delete rows in a table

  * Create, replace, alter, and drop objects

  * Control access to the database and its objects

  * Guarantee database consistency and integrity

### PL/SQL
1. PL/SQL is a procedural extension to Oracle SQL; it's a procedural language using SQL schema object to solve specific question and task.

<hr />

## <a name = "transaction">Transaction Management<a />

<hr>

### Transaction

1. A transaction is a logical, atomic unit of work that contains one or more SQL statements.

2. A transaction is defined as `a set of SQL statements with one commit statement`. If a hardware failure prevents a statement in the transaction from executing, then the other statements must be rolled back.  `The basic principle of a transaction is "all or nothing"`: an atomic operation succeeds or fails as a whole.

### Concurrency Control

1. A requirement of a multiuser RDBMS is the control of concurrency, which is the simultaneous access of the same data by multiple users.

2. Oracle Database uses `locks` to control concurrent access to data. A lock is a mechanism that prevents destructive interaction between transactions accessing a shared resource. Locks help ensure data integrity while allowing maximum concurrent access to data.

### Data Consistency

1. In Oracle Database, each user must see a consistent view of the data, including visible changes made by a user's own transactions and committed transactions of other users. For example, the database must not permit a dirty read, which occurs when one transaction sees uncommitted changes made by another concurrent transaction.

<hr />

## <a name = "Architecture">Architecture<a />

<hr />

### Database and Instance

1. A `database` is a set of files, `located on disk`, that store data. These files can exist independently of a database instance

2. An `instance` is `a set of memory structures` that manage database files. The instance consists of a shared memory area, called the system global area (SGA), and a set of background processes. An instance can exist independently of database files.

### Database Storage Structures

1. Physical Storage Structure:The physical database structures are the files that store the data. When you execute the SQL command CREATE DATABASE, the following files are created:
  * Data files:The `data` of logical database structures, such as tables and indexes, is physically stored in the data files.

  * Control files: Save the `database name` and the names and `locations` of the database files.

  * Online redo log files:An online redo log is made up of redo entries (also called redo records), which `record all changes made to data`.

2. Logical Storage Structures
  * Data blocks:At the finest level of granularity, Oracle Database data is stored in data blocks. One data block corresponds to a specific number of bytes on disk.

  * Extents:An extent is a specific number of logically contiguous data blocks, obtained in a single allocation, used to store a specific type of information.

  * Segments:A segment is a set of extents allocated for a user object (for example, a table or index), undo data, or temporary data.

  * Tablespaces:A database is divided into logical storage units called tablespaces. A tablespace is the logical container for a segment. Each tablespace contains at least one data file.

  ![assets/images/logical_storage.png](/Users/harrychen/Desktop/newlifehaonan.github.io/assets/images/logical_storage.png)
