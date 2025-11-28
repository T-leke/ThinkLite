# Architecture overview

ThinkLite is designed as a modular, layered database system built entirely in Rust.
Its architecture mirrors real-world database engines, while remaining small and understandable for learning and experimentation.

ThinkLite is organized into four major layers:

+-----------------------------------------------------------+
|                    ThinkLite CLI / RPC (future)           |
+------------------------------+----------------------------+
|         SQL Parser & Planner | Execution Engine           |
+------------------------------+----------------------------+
|                Catalog & Metadata Management              |
+------------------------------+----------------------------+
|                  Storage Engine (Pages & Files)           |
+-----------------------------------------------------------+

Each layer is implemented as its own Rust module or crate to promote clarity, testability, and extensibility.

1. Storage Engine

The storage engine is the foundation of ThinkLite.
It provides the low-level mechanisms for storing, retrieving, and updating data on disk.

Core components

File Manager — Handles reading/writing database files.

Page Manager — Allocates, loads, and flushes fixed-size pages (e.g., 4 KB).

Record Format — Encodes/decodes rows in binary form.

Table Heap — Append-only structure storing table data across pages.

Index Structures (future) — B+ Tree or Hash index for faster lookups.

Goals

Minimal, efficient, and transparent design

Durable on-disk layout

Clear separation between logical tables and physical pages

2. Catalog & Metadata Layer

The catalog is the “brain” of ThinkLite. It tracks all database objects:

Tables

Columns & types

Indexes

Root pages & storage metadata

This layer provides lookup and validation for SQL operations and acts as the bridge between SQL-level names and storage-level structures.

3. SQL Parser, AST, and Planner

This layer transforms raw SQL strings into executable plans.

Components
Lexer

Converts SQL text into tokens.

Parser

Builds an AST (Abstract Syntax Tree) for supported statements:

CREATE TABLE

INSERT INTO

SELECT

Simple WHERE clauses

Logical Planner

Transforms the AST into a logical plan representing relational operators:

SeqScan

Projection

Filter

(future) IndexScan

This layer defines what the database should do, abstracting away storage details.

4. Execution Engine

The execution engine runs the logical plan and produces results.

Operators

Sequence Scan — Read all rows from the table heap.

Filter — Apply predicates.

Projection — Select specific columns.

Index Scan (future) — Seek rows using a B+ Tree.

Execution Flow

Receive logical plan

Build an operator tree

Execute pipeline

Return rows or update results

This architecture mirrors real relational engines and teaches concepts like iterators, operator trees, and pipelined execution.

5. CLI Layer (thinklite-cli)

The CLI acts as a simple interactive shell for ThinkLite.

Features:

Local mode opening .tlite database files

REPL for running SQL commands

Metadata commands (.tables, .schema)

Switch to remote mode when distributed features arrive

The CLI is intentionally kept thin, delegating all logic to thinklite-core.

6. Distributed Layer (Future)

When ThinkLite evolves beyond single-node mode, two additional crates come into play:

thinklite-net

Implements:

RPC protocol

Request/response types

Remote execution API

thinklite-node

Runs ThinkLite as a network-accessible database server:

Hosts a ThinkLiteDB instance

Receives queries via RPC

Handles replication or sharding logic

Long-term distributed goals:

Leader/follower replication (WAL shipping)

Horizontal sharding

Coordinator for routing queries

The distributed layer sits above the engine and reuses all core modules without modification.

7. AI Agent Layer (Future Phase)

The AI layer provides automated insights and natural language query capabilities.
It uses ThinkLite as a tool through exposed APIs.

Capabilities:

Schema introspection

Natural-language → SQL translation

Insight generation (summaries, anomalies, trends)

Optional plotting or analytics extensions

This layer is intentionally decoupled from the database engine and interacts through a stable API surface.