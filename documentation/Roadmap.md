# Project Roadmap

ThinkLite is built brick by brick to learn the full lifecycle of database and distributed system design.
This roadmap outlines the major development phases from a single-node storage engine to a distributed and intelligent data system.

📌 Phase 0 — Project Scaffolding & Setup

Status: Planned

Set up Rust workspace (thinklite-core, thinklite-cli)

Configure rustfmt, clippy, test workflow

Initial documentation: README, DEVLOG, architecture notes

📌 Phase 1 — Storage Engine Foundations

Status: Planned
Build the minimal database core capable of storing and retrieving rows.

Page format and fixed-size storage layout

Page manager (allocate, read, write pages)

Record encoding/decoding (INT, TEXT)

Table heap (append-only storage)

Basic catalog for storing table metadata

Outcome: Insert and scan rows using Rust API.

📌 Phase 2 — Minimal SQL Layer

Status: Planned
Introduce a SQL frontend and interpreter.

SQL lexer and tokenizer

SQL parser (AST for CREATE, INSERT, SELECT)

Logical plan builder

Execution engine (SeqScan, Filter, Projection)

Public API: ThinkLiteDB::execute(sql)

Outcome: Execute simple SQL queries against a single-node database.

📌 Phase 3 — Command Line Interface

Status: Planned
Create a lightweight REPL similar to sqlite3.

Read–eval–print loop

Pretty-print query results

Table inspection commands (.tables, .schema)

Local file mode (open .tlite file)

Outcome: Fully usable local database from the terminal.

📌 Phase 4 — Indexing & Query Optimization

Status: Planned
Improve query performance and introduce indexing.

B+ Tree or Hash index implementation

Catalog integration for indexes

Index-based query execution

Improved WHERE clause support

Optional ORDER BY and LIMIT support

Outcome: Faster and more expressive SQL queries.

📌 Phase 5 — Transactions & Durability (Optional but Valuable)

Status: Planned
Add durability and simple ACID behavior.

Write-ahead log (WAL)

Recovery on startup

Basic locking model

BEGIN, COMMIT, and ROLLBACK

Outcome: Safer writes and crash recovery.

📌 Phase 6 — Distributed ThinkLite

Status: Future
Extend ThinkLite from a local DB to a distributed system.

6.1 RPC Layer (thinklite-net)

Query request/response protocol

Client & server RPC library

6.2 Node Process (thinklite-node)

Standalone server running ThinkLiteDB

Remote execution of SQL

CLI support for remote queries

6.3 Replication

Leader/follower model

WAL shipping

Read replicas

6.4 Sharding

Partition tables or keys across nodes

Routing/coordinator component

Outcome: A simple, horizontally scalable distributed database.

📌 Phase 7 — Intelligent Insights Layer (Agent)

Status: Future
Introduce an AI agent that interacts with ThinkLite as a tool.

Schema introspection tools

Natural-language → SQL generation

Automated insights (summaries, anomalies, trends)

Optional visualization hooks

Outcome: ThinkLite becomes an intelligent analytical database.

📌 Phase 8 — Documentation, Testing & Hardening

Status: Ongoing

Unit tests across all modules

Integration tests for SQL queries

Internal architecture docs

Performance benchmarks

Packaging and releases

Outcome: A polished, maintainable, and well-documented system.