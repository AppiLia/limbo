# Compatibility with SQLite

This document describes the compatibility of Limbo with SQLite.

## Table of contents:

- [Features](#features)
- [SQLite query language](#sqlite-query-language)
    - [Statements](#statements)
      - [PRAGMA Statements](#pragma)
    - [Expressions](#expressions)
    - [Functions](#functions)
- [SQLite C API](#sqlite-c-api)
- [SQLite VDBE opcodes](#sqlite-vdbe-opcodes)
- [Extensions](#extensions)
    - [UUID](#uuid)

## Features

Limbo aims to be fully compatible with SQLite, with opt-in features not supported by SQLite.

The current status of Limbo is:

* ✅ SQLite file format is fully supported.
* 🚧 SQLite query language [[status](#sqlite-query-language)]
* 🚧 SQLite C API [[status](#sqlite-c-api)].
* ⛔️ Concurrent access from multiple processes is not supported.

## SQLite query language

### Statements

| Statement                 | Status  | Comment |
| ------------------------- | ------- | ------- |
| ALTER TABLE               | No      |         |
| ANALYZE                   | No      |         |
| ATTACH DATABASE           | No      |         |
| BEGIN TRANSACTION         | No      |         |
| COMMIT TRANSACTION        | No      |         |
| CREATE INDEX              | No      |         |
| CREATE TABLE              | Partial |         |
| CREATE TRIGGER            | No      |         |
| CREATE VIEW               | No      |         |
| CREATE VIRTUAL TABLE      | No      |         |
| DELETE                    | No      |         |
| DETACH DATABASE           | No      |         |
| DROP INDEX                | No      |         |
| DROP TABLE                | No      |         |
| DROP TRIGGER              | No      |         |
| DROP VIEW                 | No      |         |
| END TRANSACTION           | No      |         |
| EXPLAIN                   | Yes     |         |
| INDEXED BY                | No      |         |
| INSERT                    | Partial |         |
| ON CONFLICT clause        | No      |         |
| REINDEX                   | No      |         |
| RELEASE SAVEPOINT         | No      |         |
| REPLACE                   | No      |         |
| RETURNING clause          | No      |         |
| ROLLBACK TRANSACTION      | No      |         |
| SAVEPOINT                 | No      |         |
| SELECT                    | Yes     |         |
| SELECT ... WHERE          | Yes     |         |
| SELECT ... WHERE ... LIKE | Yes     |         |
| SELECT ... LIMIT          | Yes     |         |
| SELECT ... ORDER BY       | Yes     |         |
| SELECT ... GROUP BY       | Yes     |         |
| SELECT ... HAVING         | Yes     |         |
| SELECT ... JOIN           | Yes     |         |
| SELECT ... CROSS JOIN     | Yes     | SQLite CROSS JOIN means "do not reorder joins". We don't support that yet anyway. |
| SELECT ... INNER JOIN     | Yes     |         |
| SELECT ... OUTER JOIN     | Partial | no RIGHT JOIN |
| SELECT ... JOIN USING     | Yes     |         |
| SELECT ... NATURAL JOIN   | Yes     |         |
| UPDATE                    | No      |         |
| UPSERT                    | No      |         |
| VACUUM                    | No      |         |
| WITH clause               | No      |         |

#### [PRAGMA](https://www.sqlite.org/pragma.html)


| Statement                        | Status     | Comment                                         |
|----------------------------------|------------|-------------------------------------------------|
| PRAGMA analysis_limit            | No         |                                                 |
| PRAGMA application_id            | No         |                                                 |
| PRAGMA auto_vacuum               | No         |                                                 |
| PRAGMA automatic_index           | No         |                                                 |
| PRAGMA busy_timeout              | No         |                                                 |
| PRAGMA busy_timeout              | No         |                                                 |
| PRAGMA cache_size                | Yes        |                                                 |
| PRAGMA cache_spill               | No         |                                                 |
| PRAGMA case_sensitive_like       | Not Needed | deprecated in SQLite                            |
| PRAGMA cell_size_check           | No         |                                                 |
| PRAGMA checkpoint_fullsync       | No         |                                                 |
| PRAGMA collation_list            | No         |                                                 |
| PRAGMA compile_options           | No         |                                                 |
| PRAGMA count_changes             | Not Needed | deprecated in SQLite                            |
| PRAGMA data_store_directory      | Not Needed | deprecated in SQLite                            |
| PRAGMA data_version              | No         |                                                 |
| PRAGMA database_list             | No         |                                                 |
| PRAGMA default_cache_size        | Not Needed | deprecated in SQLite                            |
| PRAGMA defer_foreign_keys        | No         |                                                 |
| PRAGMA empty_result_callbacks    | Not Needed | deprecated in SQLite                            |
| PRAGMA encoding                  | No         |                                                 |
| PRAGMA foreign_key_check         | No         |                                                 |
| PRAGMA foreign_key_list          | No         |                                                 |
| PRAGMA foreign_keys              | No         |                                                 |
| PRAGMA freelist_count            | No         |                                                 |
| PRAGMA full_column_names         | Not Needed | deprecated in SQLite                            |
| PRAGMA fullsync                  | No         |                                                 |
| PRAGMA function_list             | No         |                                                 |
| PRAGMA hard_heap_limit           | No         |                                                 |
| PRAGMA ignore_check_constraints  | No         |                                                 |
| PRAGMA incremental_vacuum        | No         |                                                 |
| PRAGMA index_info                | No         |                                                 |
| PRAGMA index_list                | No         |                                                 |
| PRAGMA index_xinfo               | No         |                                                 |
| PRAGMA integrity_check           | No         |                                                 |
| PRAGMA journal_mode              | No         |                                                 |
| PRAGMA journal_size_limit        | No         |                                                 |
| PRAGMA legacy_alter_table        | No         |                                                 |
| PRAGMA legacy_file_format        | No         |                                                 |
| PRAGMA locking_mode              | No         |                                                 |
| PRAGMA max_page_count            | No         |                                                 |
| PRAGMA mmap_size                 | No         |                                                 |
| PRAGMA module_list               | No         |                                                 |
| PRAGMA optimize                  | No         |                                                 |
| PRAGMA page_count                | No         |                                                 |
| PRAGMA page_size                 | No         |                                                 |
| PRAGMA parser_trace              | No         |                                                 |
| PRAGMA pragma_list               | No         |                                                 |
| PRAGMA query_only                | No         |                                                 |
| PRAGMA quick_check               | No         |                                                 |
| PRAGMA read_uncommitted          | No         |                                                 |
| PRAGMA recursive_triggers        | No         |                                                 |
| PRAGMA reverse_unordered_selects | No         |                                                 |
| PRAGMA schema_version            | No         |                                                 |
| PRAGMA secure_delete             | No         |                                                 |
| PRAGMA short_column_names        | Not Needed | deprecated in SQLite                            |
| PRAGMA shrink_memory             | No         |                                                 |
| PRAGMA soft_heap_limit           | No         |                                                 |
| PRAGMA stats                     | No         | Used for testing in SQLite                      |
| PRAGMA synchronous               | No         |                                                 |
| PRAGMA table_info                | No         |                                                 |
| PRAGMA table_list                | No         |                                                 |
| PRAGMA table_xinfo               | No         |                                                 |
| PRAGMA temp_store                | No         |                                                 |
| PRAGMA temp_store_directory      | Not Needed | deprecated in SQLite                            |
| PRAGMA threads                   | No         |                                                 |
| PRAGMA trusted_schema            | No         |                                                 |
| PRAGMA user_version              | No         |                                                 |
| PRAGMA vdbe_addoptrace           | No         |                                                 |
| PRAGMA vdbe_debug                | No         |                                                 |
| PRAGMA vdbe_listing              | No         |                                                 |
| PRAGMA vdbe_trace                | No         |                                                 |
| PRAGMA wal_autocheckpoint        | No         |                                                 |
| PRAGMA wal_checkpoint            | Partial    | Not supported calling with param (pragma-value) |
| PRAGMA writable_schema           | No         |                                                 |

### Expressions

Feature support of [sqlite expr syntax](https://www.sqlite.org/lang_expr.html).

| Syntax                       | Status  | Comment |
|------------------------------|---------|---------|
| literals                     | Yes     |         |
| schema.table.column          | Partial | Schemas aren't supported |
| unary operator               | Yes     | |
| binary operator              | Partial | Only `%`, `!<`, and `!>` are unsupported |
| agg() FILTER (WHERE ...)     | No      | Is incorrectly ignored |
| ... OVER (...)               | No      | Is incorrectly ignored |
| (expr)                       | Yes     |         |
| CAST (expr AS type)          | Yes     |         |
| COLLATE                      | No      |         |
| (NOT) LIKE                   | No      |         |
| (NOT) GLOB                   | No      |         |
| (NOT) REGEXP                 | No      |         |
| (NOT) MATCH                  | No      |         |
| IS (NOT)                     | No      |         |
| IS (NOT) DISTINCT FROM       | No      |         |
| (NOT) BETWEEN ... AND ...    | No      |         |
| (NOT) IN (subquery)          | No      |         |
| (NOT) EXISTS (subquery)      | No      |         |
| CASE WHEN THEN ELSE END      | Yes     |         |
| RAISE                        | No      |         |

### SQL functions

#### Scalar functions

| Function                     | Status | Comment |
|------------------------------|--------|---------|
| abs(X)                       | Yes    |         |
| changes()                    | Partial| Still need to support update statements and triggers |
| char(X1,X2,...,XN)           | Yes    |         |
| coalesce(X,Y,...)            | Yes    |         |
| concat(X,...)                | Yes    |         |
| concat_ws(SEP,X,...)         | Yes    |         |
| format(FORMAT,...)           | No     |         |
| glob(X,Y)                    | Yes    |         |
| hex(X)                       | Yes    |         |
| ifnull(X,Y)                  | Yes    |         |
| iif(X,Y,Z)                   | Yes    |         |
| instr(X,Y)                   | Yes    |         |
| last_insert_rowid()          | Yes    |         |
| length(X)                    | Yes    |         |
| like(X,Y)                    | Yes    |         |
| like(X,Y,Z)                  | Yes    |         |
| likelihood(X,Y)              | No     |         |
| likely(X)                    | No     |         |
| load_extension(X)            | Yes    | sqlite3 extensions not yet supported |
| load_extension(X,Y)          | No     |         |
| lower(X)                     | Yes    |         |
| ltrim(X)                     | Yes    |         |
| ltrim(X,Y)                   | Yes    |         |
| max(X,Y,...)                 | Yes    |         |
| min(X,Y,...)                 | Yes    |         |
| nullif(X,Y)                  | Yes    |         |
| octet_length(X)              | Yes    |         |
| printf(FORMAT,...)           | No     |         |
| quote(X)                     | Yes    |         |
| random()                     | Yes    |         |
| randomblob(N)                | Yes    |         |
| replace(X,Y,Z)               | Yes    |         |
| round(X)                     | Yes    |         |
| round(X,Y)                   | Yes    |         |
| rtrim(X)                     | Yes    |         |
| rtrim(X,Y)                   | Yes    |         |
| sign(X)                      | Yes    |         |
| soundex(X)                   | Yes    |         |
| sqlite_compileoption_get(N)  | No     |         |
| sqlite_compileoption_used(X) | No     |         |
| sqlite_offset(X)             | No     |         |
| sqlite_source_id()           | No     |         |
| sqlite_version()             | Yes    |         |
| substr(X,Y,Z)                | Yes    |         |
| substr(X,Y)                  | Yes    |         |
| substring(X,Y,Z)             | Yes    |         |
| substring(X,Y)               | Yes    |         |
| total_changes()              | Partial| Still need to support update statements and triggers |
| trim(X)                      | Yes    |         |
| trim(X,Y)                    | Yes    |         |
| typeof(X)                    | Yes    |         |
| unhex(X)                     | Yes    |         |
| unhex(X,Y)                   | Yes    |         |
| unicode(X)                   | Yes    |         |
| unlikely(X)                  | No     |         |
| upper(X)                     | Yes    |         |
| zeroblob(N)                  | Yes    |         |

#### Mathematical functions

| Function   | Status | Comment |
| ---------- | ------ | ------- |
| acos(X)    | Yes    |         |
| acosh(X)   | Yes    |         |
| asin(X)    | Yes    |         |
| asinh(X)   | Yes    |         |
| atan(X)    | Yes    |         |
| atan2(Y,X) | Yes    |         |
| atanh(X)   | Yes    |         |
| ceil(X)    | Yes    |         |
| ceiling(X) | Yes    |         |
| cos(X)     | Yes    |         |
| cosh(X)    | Yes    |         |
| degrees(X) | Yes    |         |
| exp(X)     | Yes    |         |
| floor(X)   | Yes    |         |
| ln(X)      | Yes    |         |
| log(B,X)   | Yes    |         |
| log(X)     | Yes    |         |
| log10(X)   | Yes    |         |
| log2(X)    | Yes    |         |
| mod(X,Y)   | Yes    |         |
| pi()       | Yes    |         |
| pow(X,Y)   | Yes    |         |
| power(X,Y) | Yes    |         |
| radians(X) | Yes    |         |
| sin(X)     | Yes    |         |
| sinh(X)    | Yes    |         |
| sqrt(X)    | Yes    |         |
| tan(X)     | Yes    |         |
| tanh(X)    | Yes    |         |
| trunc(X)   | Yes    |         |

#### Aggregate functions

| Function                     | Status  | Comment |
|------------------------------|---------|---------|
| avg(X)                       | Yes     |         |
| count()                      | Yes     |         |
| count(*)                     | Yes     |         |
| group_concat(X)              | Yes     |         |
| group_concat(X,Y)            | Yes     |         |
| string_agg(X,Y)              | Yes     |         |
| max(X)                       | Yes     |         |
| min(X)                       | Yes     |         |
| sum(X)                       | Yes     |         |
| total(X)                     | Yes     |         |

#### Date and time functions

| Function    | Status  | Comment                      |
|-------------|---------|------------------------------|
| date()      | Yes     | partially supports modifiers |
| time()      | Yes     | partially supports modifiers |
| datetime()  | Yes     | partially supports modifiers |
| julianday() | Partial | does not support modifiers   |
| unixepoch() | Partial | does not support modifiers   |
| strftime()  | No      |                              |
| timediff()  | No      |                              |

Modifiers:

|  Modifier      | Status|  Comment                        |
|----------------|-------|---------------------------------|
| Days           | Yes 	 |                                 |
| Hours          | Yes	 |                                 |
| Minutes        | Yes	 |                                 |
| Seconds        | Yes	 |                                 |
| Months         | Yes	 |                                 |
| Years          | Yes	 |                                 |
| TimeOffset     | Yes	 |                                 |
| DateOffset	 | Yes   |                                 |
| DateTimeOffset | Yes   |                                 |
| Ceiling	     | No    |                                 |
| Floor          | No    |                                 |
| StartOfMonth	 | Yes	 |                                 |
| StartOfYear	 | Yes	 |                                 |
| StartOfDay	 | Yes	 |                                 |
| Weekday(N)	 | Yes   |                                 |
| Auto           | No    |                                 |
| UnixEpoch      | No    |                                 |
| JulianDay      | No    |                                 |
| Localtime      |Partial| requires fixes to avoid double conversions.|
| Utc            |Partial| requires fixes to avoid double conversions.|
| Subsec         | Yes   |                                  |

#### JSON functions

| Function                           | Status  | Comment                                                                                                                                      |
|------------------------------------|---------|----------------------------------------------------------------------------------------------------------------------------------------------|
| json(json)                         | Partial |                                                                                                                                              |
| jsonb(json)                        |         |                                                                                                                                              |
| json_array(value1,value2,...)      | Yes     |                                                                                                                                              |
| jsonb_array(value1,value2,...)     |         |                                                                                                                                              |
| json_array_length(json)            | Yes     |                                                                                                                                              |
| json_array_length(json,path)       | Yes     |                                                                                                                                              |
| json_error_position(json)          | Yes     |                                                                                                                                              |
| json_extract(json,path,...)        | Partial | Does not fully support unicode literal syntax and does not allow numbers > 2^127 - 1 (which SQLite truncates to i32), does not support BLOBs |
| jsonb_extract(json,path,...)       |         |                                                                                                                                              |
| json -> path                       | Yes     |                                                                                                                                              |
| json ->> path                      | Yes     |                                                                                                                                              |
| json_insert(json,path,value,...)   |         |                                                                                                                                              |
| jsonb_insert(json,path,value,...)  |         |                                                                                                                                              |
| json_object(label1,value1,...)     | Yes     | When keys are duplicated, only the last one processed is returned. This differs from sqlite, where the keys in the output can be duplicated  |
| jsonb_object(label1,value1,...)    |         |                                                                                                                                              |
| json_patch(json1,json2)            |         |                                                                                                                                              |
| jsonb_patch(json1,json2)           |         |                                                                                                                                              |
| json_pretty(json)                  |         |                                                                                                                                              |
| json_remove(json,path,...)         |         |                                                                                                                                              |
| jsonb_remove(json,path,...)        |         |                                                                                                                                              |
| json_replace(json,path,value,...)  |         |                                                                                                                                              |
| jsonb_replace(json,path,value,...) |         |                                                                                                                                              |
| json_set(json,path,value,...)      |         |                                                                                                                                              |
| jsonb_set(json,path,value,...)     |         |                                                                                                                                              |
| json_type(json)                    | Yes     |                                                                                                                                              |
| json_type(json,path)               | Yes     |                                                                                                                                              |
| json_valid(json)                   |         |                                                                                                                                              |
| json_valid(json,flags)             |         |                                                                                                                                              |
| json_quote(value)                  |         |                                                                                                                                              |
| json_group_array(value)            |         |                                                                                                                                              |
| jsonb_group_array(value)           |         |                                                                                                                                              |
| json_group_object(label,value)     |         |                                                                                                                                              |
| jsonb_group_object(name,value)     |         |                                                                                                                                              |
| json_each(json)                    |         |                                                                                                                                              |
| json_each(json,path)               |         |                                                                                                                                              |
| json_tree(json)                    |         |                                                                                                                                              |
| json_tree(json,path)               |         |                                                                                                                                              |

## SQLite C API

| Interface           | Status  | Comment |
|---------------------|---------|---------|
| sqlite3_open        | Partial |         |
| sqlite3_close       | Yes     |         |
| sqlite3_prepare     | Partial |         |
| sqlite3_finalize    | Yes     |         |
| sqlite3_step        | Yes     |         |
| sqlite3_column_text | Yes     |         |

## SQLite VDBE opcodes

| Opcode         | Status |
|----------------|--------|
| Add            | Yes    |
| AddImm         | No     |
| Affinity       | No     |
| AggFinal       | Yes    |
| AggStep        | Yes    |
| AggStep        | Yes    |
| And            | No     |
| AutoCommit     | No     |
| BitAnd         | Yes    |
| BitNot         | Yes    |
| BitOr          | Yes    |
| Blob           | Yes    |
| Checkpoint     | No     |
| Clear          | No     |
| Close          | No     |
| CollSeq        | No     |
| Column         | Yes    |
| Compare        | Yes    |
| Concat         | Yes    |
| Copy           | Yes    |
| Count          | No     |
| CreateIndex    | No     |
| CreateTable    | No     |
| DecrJumpZero   | Yes    |
| Delete         | No     |
| Destroy        | No     |
| Divide         | Yes    |
| DropIndex      | No     |
| DropTable      | No     |
| DropTrigger    | No     |
| EndCoroutine   | Yes    |
| Eq             | Yes    |
| Expire         | No     |
| Explain        | No     |
| FkCounter      | No     |
| FkIfZero       | No     |
| Found          | No     |
| Function       | Yes    |
| Ge             | Yes    |
| Gosub          | Yes    |
| Goto           | Yes    |
| Gt             | Yes    |
| Halt           | Yes    |
| HaltIfNull     | No     |
| IdxDelete      | No     |
| IdxGE          | Yes    |
| IdxInsert      | No     |
| IdxLT          | No     |
| IdxRowid       | No     |
| If             | Yes    |
| IfNeg          | No     |
| IfNot          | Yes    |
| IfPos          | Yes    |
| IfZero         | No     |
| IncrVacuum     | No     |
| Init           | Yes    |
| InitCoroutine  | Yes    |
| Insert         | No     |
| InsertAsync    | Yes    |
| InsertAwait    | Yes    |
| InsertInt      | No     |
| Int64          | No     |
| Integer        | Yes    |
| IntegrityCk    | No     |
| IsNull         | Yes    |
| IsUnique       | No     |
| JournalMode    | No     |
| Jump           | Yes    |
| Last           | No     |
| Le             | Yes    |
| LoadAnalysis   | No     |
| Lt             | Yes    |
| MakeRecord     | Yes    |
| MaxPgcnt       | No     |
| MemMax         | No     |
| Move           | No     |
| Multiply       | Yes    |
| MustBeInt      | Yes    |
| Ne             | Yes    |
| NewRowid       | Yes    |
| Next           | No     |
| NextAsync      | Yes    |
| NextAwait      | Yes    |
| Noop           | No     |
| Not            | Yes    |
| NotExists      | Yes    |
| NotFound       | No     |
| NotNull        | Yes    |
| Null           | Yes    |
| NullRow        | Yes    |
| Once           | No     |
| OpenAutoindex  | No     |
| OpenEphemeral  | No     |
| OpenPseudo     | Yes    |
| OpenRead       | Yes    |
| OpenReadAsync  | Yes    |
| OpenWrite      | No     |
| OpenWriteAsync | Yes    |
| OpenWriteAwait | Yes    |
| Or             | No     |
| Pagecount      | No     |
| Param          | No     |
| ParseSchema    | No     |
| Permutation    | No     |
| Prev           | No     |
| PrevAsync      | Yes    |
| PrevAwait      | Yes    |
| Program        | No     |
| ReadCookie     | No     |
| Real           | Yes    |
| RealAffinity   | Yes    |
| Remainder      | Yes    |
| ResetCount     | No     |
| ResultRow      | Yes    |
| Return         | Yes    |
| Rewind         | Yes    |
| RewindAsync    | Yes    |
| RewindAwait    | Yes    |
| RowData        | No     |
| RowId          | Yes    |
| RowKey         | No     |
| RowSetAdd      | No     |
| RowSetRead     | No     |
| RowSetTest     | No     |
| Rowid          | Yes    |
| SCopy          | No     |
| Savepoint      | No     |
| Seek           | No     |
| SeekGe         | Yes    |
| SeekGt         | Yes    |
| SeekLe         | No     |
| SeekLt         | No     |
| SeekRowid      | Yes    |
| Sequence       | No     |
| SetCookie      | No     |
| ShiftLeft      | Yes    |
| ShiftRight     | Yes    |
| SoftNull       | Yes    |
| Sort           | No     |
| SorterCompare  | No     |
| SorterData     | Yes    |
| SorterInsert   | Yes    |
| SorterNext     | Yes    |
| SorterOpen     | Yes    |
| SorterSort     | Yes    |
| String         | No     |
| String8        | Yes    |
| Subtract       | Yes    |
| TableLock      | No     |
| ToBlob         | No     |
| ToInt          | No     |
| ToNumeric      | No     |
| ToReal         | No     |
| ToText         | No     |
| Trace          | No     |
| Transaction    | Yes    |
| VBegin         | No     |
| VColumn        | No     |
| VCreate        | No     |
| VDestroy       | No     |
| VFilter        | No     |
| VNext          | No     |
| VOpen          | No     |
| VRename        | No     |
| VUpdate        | No     |
| Vacuum         | No     |
| Variable       | No     |
| VerifyCookie   | No     |
| Yield          | Yes    |
| ZeroOrNull     | Yes    |

##  Extensions

Limbo has in-tree extensions.

### UUID

UUID's in Limbo are `blobs` by default.

| Function              | Status | Comment                                                       | 
|-----------------------|--------|---------------------------------------------------------------|
| uuid4()               | Yes    | UUID version 4                                                |
| uuid4_str()           | Yes    | UUID v4 string alias `gen_random_uuid()` for PG compatibility |
| uuid7(X?)             | Yes    | UUID version 7 (optional parameter for seconds since epoch)   |
| uuid7_timestamp_ms(X) | Yes    | Convert a UUID v7 to milliseconds since epoch                 |
| uuid_str(X)           | Yes    | Convert a valid UUID to string                                |
| uuid_blob(X)          | Yes    | Convert a valid UUID to blob                                  |

### regexp

The `regexp` extension is compatible with [sqlean-regexp](https://github.com/nalgeon/sqlean/blob/main/docs/regexp.md).

| Function                                       | Status | Comment | 
|------------------------------------------------|--------|---------|
| regexp(pattern, source)                        | Yes    |         |
| regexp_like(source, pattern)                   | Yes    |         |
| regexp_substr(source, pattern)                 | Yes    |         |
| regexp_capture(source, pattern[, n])           | No     |         |
| regexp_replace(source, pattern, replacement)   | No     |         |
