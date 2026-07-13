# Agentic ERP Assistant

*A proof of concept for enterprise ERP automation using Claude tool use and PostgreSQL.*

## Overview

This project explores how a large language model can function as an **AI agent** for interacting with an ERP system through structured tools instead of directly generating SQL or relying on UI automation.

The prototype simulates an ERP environment for **ABD Druckluft GmbH**, a German compressed air systems company. The database schema was designed as a realistic approximation of an ERP system based on publicly available information about the company's business processes and services.

Rather than treating the language model as a chatbot, this project investigates how an agent can reason about an unfamiliar database, inspect its structure, safely execute operations, and communicate results in natural language.

---

## Motivation

Large Language Models have become increasingly capable of understanding business processes and natural language. However, enterprise software requires more than conversational ability:

* data integrity
* transactional safety
* schema awareness
* explainability
* controlled execution of write operations

Instead of allowing the model to freely generate SQL, this project explores an **agentic workflow** in which the model interacts with the database exclusively through well-defined tools.

The objective is to demonstrate how AI agents can become reliable interfaces for ERP systems while maintaining the safeguards expected in enterprise software.

---

## Architecture

<img src="docs/architecture.svg" width="450"/>

The language model never communicates with PostgreSQL directly.

Instead, it operates through a constrained set of tools that expose only the functionality required to inspect the schema and perform database operations safely.

---

## ERP Modules

The prototype database contains representative ERP modules, including:

* Customer Relationship Management (CRM)
* Products & Inventory
* Service Orders
* Projects
* Rentals
* Purchasing
* Invoicing
* Time Tracking
* Support Tickets
* Document Management

The schema is intentionally broad enough to require the agent to reason across multiple business domains instead of answering isolated SQL questions.

---

## Agent Workflow

### Read Operations

For information requests, the agent follows these steps:

1. Discover available tables.
2. Inspect the required schema.
3. Generate an appropriate SQL query.
4. Execute the query.
5. Explain the results in natural language.

Because the agent reasons over the schema instead of relying on hardcoded knowledge, it can adapt to previously unseen database structures.

---

### Write Operations

For INSERT, UPDATE, and DELETE operations, the workflow is intentionally more conservative.

The agent:

1. inspects the schema,
2. retrieves PostgreSQL enum values when required,
3. performs a **dry run** inside a transaction,
4. rolls the transaction back,
5. presents the expected result,
6. commits only after explicit user confirmation.

This approach reduces the risk of accidental modifications while preserving a natural conversational interface.

---

## Design Decisions

### Tool-Based Reasoning

Instead of directly asking the language model to produce SQL, the agent first gathers information about the database through dedicated tools.

This mirrors how a human developer would approach an unfamiliar ERP system by inspecting its schema before writing queries.

---

### Read and Write Separation

Read-only queries and modifying queries are handled by separate tools.

This allows different validation rules and makes it significantly harder for unintended write operations to occur.

---

### Dry-Run Before Commit

Every write operation is executed inside a transaction that is rolled back unless explicitly confirmed by the user.

This creates a verification step before any persistent change is made and provides an additional safety layer for enterprise environments.

---

### Schema Awareness

The agent does not assume prior knowledge of the database.

Instead, it dynamically discovers:

* available tables
* column definitions
* foreign key relationships
* PostgreSQL enum values

This makes the approach significantly more flexible than prompt-engineered SQL generation alone.

---

## Example Questions

The agent can answer questions such as:

* Which service orders are currently overdue?
* How much revenue has been invoiced in 2025?
* Which customers require maintenance?
* How many hours has an employee logged this year?
* Create a new customer record.
* Update an existing project.
* Insert a purchase order.

Write operations require confirmation before changes are committed.

---

## Technology Stack

* Python
* Anthropic Claude (Tool Use API)
* PostgreSQL

---

## Current Limitations

This project is intended as an architectural proof of concept rather than a production-ready ERP assistant.

Current limitations include:

* direct PostgreSQL access instead of ERP APIs
* no authentication or role-based permissions
* single-agent architecture
* no retrieval over external documentation
* limited error recovery
* no long-term memory or planning across sessions

---

## Future Work

Potential extensions include:

* replacing direct database access with ERP REST APIs
* integration with SAP, Microsoft Dynamics, or Odoo
* multi-agent orchestration for complex workflows
* Retrieval-Augmented Generation (RAG) over ERP documentation
* approval workflows for business-critical operations
* audit logging and role-aware permissions
* autonomous planning across multiple business processes

---

## Purpose

The purpose of this project is to explore how **agentic AI systems** can interact safely with enterprise software.

Rather than demonstrating text-to-SQL generation, the project focuses on **tool-based reasoning**, **controlled execution**, and **enterprise-oriented safety mechanisms** that could form the foundation of future AI assistants for ERP systems.
