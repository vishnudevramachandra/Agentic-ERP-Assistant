# Agentic ERP Assistant

*An AI agent that interacts with an ERP system through structured tools and transactional database operations.*

## Overview

This project explores how a Large Language Model can function as an **ERP assistant** by reasoning over a database schema, generating SQL dynamically, and interacting with PostgreSQL exclusively through controlled tools.

Unlike traditional text-to-SQL systems, the language model never communicates with the database directly. Instead, every database operation is performed through a constrained tool interface that controls how data is read and modified.

The prototype simulates an ERP system for **ABD Druckluft GmbH**, a German compressed air systems company. The schema was designed as a realistic approximation of an ERP environment based on publicly available information about the company's business.

---

## Features

* Natural language interface for ERP data
* Dynamic database schema discovery
* Automatic SQL generation
* Tool-based reasoning
* Safe transactional write operations
* Human approval before database commits
* PostgreSQL backend
* Anthropic Claude Tool Use API

---

## Architecture

<img src="docs/architecture.svg" width="450"/>

The language model never communicates with PostgreSQL directly. Every interaction passes through dedicated Python tools that control schema inspection, SQL execution, transaction handling, and user approval.

---

# Demo

## Reading ERP data

The agent can inspect an unfamiliar schema, generate SQL dynamically, execute it, and explain the results in natural language.

<img src="docs/demo1.gif" alt="Demo1" width="900">

---

## Multi-step reasoning

Rather than relying on hardcoded SQL, the agent first discovers the relevant schema before generating a query.

<img src="docs/demo2.gif" alt="Demo2" width="900">

---

## Safe database modifications

Write operations are executed inside a PostgreSQL transaction.

The execution tool—not the language model—controls whether the transaction is committed.

Before any database change becomes permanent, the tool presents the affected rows and asks the user for approval.

<img src="docs/demo3.gif" alt="Demo3" width="900">

---

## 🚀 Live Demo

**Try it yourself:** [vramachandra.ddns.net/abderp](https://vramachandra.ddns.net/abderp)
for login enter: abdERP26*#?

---

## ERP Modules

The prototype database contains representative ERP modules including:

* Customer Relationship Management (CRM)
* Products & Inventory
* Projects
* Service Orders
* Rentals
* Purchasing
* Invoicing
* Time Tracking
* Support Tickets
* Document Management

The schema is intentionally broad enough that the agent must reason across multiple business domains instead of relying on hardcoded queries.

---

## Design Principles

* **Tool-based reasoning** — The language model never accesses PostgreSQL directly.
* **Schema discovery** — Tables, columns and relationships are inspected dynamically before generating SQL.
* **Transactional writes** — Database modifications execute inside a PostgreSQL transaction.
* **Human approval** — The execution tool—not the language model—controls whether a transaction is committed.

---

## Technology Stack

* Python
* PostgreSQL
* Anthropic Claude (Tool Use API)

---

## Current Limitations

This project is an architectural proof of concept rather than a production-ready ERP assistant.

Current limitations include:

* Direct PostgreSQL access instead of ERP APIs
* No authentication or role-based permissions
* Single-agent architecture
* No retrieval over external documentation
* No persistent conversational memory
* Limited error recovery

---

## Future Work

Potential future extensions include:

* Integration with SAP, Microsoft Dynamics, or Odoo
* ERP REST API integration
* Multi-agent orchestration
* Retrieval-Augmented Generation (RAG) over ERP documentation
* Role-aware permissions
* Audit logging
* Approval workflows
* Long-term planning across business processes

---

## Purpose

This project explores how **agentic AI systems** can interact safely with enterprise software.

The focus is not text-to-SQL generation, but the design of an AI agent that reasons through structured tools while keeping database execution and transaction control outside the language model.
