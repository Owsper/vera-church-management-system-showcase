# Vera — Product Decisions

Vera is a private client–server church management application designed to help
church administrators manage members, families, donations, payments, finances,
expenses, income, events, reports, and settings.

This document explains the main product and technical decisions behind the
project.

## 1. Moved from local SQLite to PostgreSQL

The first version of Vera used a local SQLite database. This was useful for rapid
prototyping, but it limited the application as it evolved into a client–server
product.

PostgreSQL was selected to provide:

- Centralized data storage
- A stronger relational data model
- Better support for structured financial records
- Improved data integrity through constraints and relationships
- A clearer foundation for multi-user and multi-church use

## 2. Separated the desktop client from the backend

Vera uses a PySide6 desktop client and a FastAPI backend. The client does not
access PostgreSQL directly; instead, it communicates with the backend through a
JSON REST API.

This separation provides several benefits:

- The user interface, business logic, and data storage remain independent.
- The backend can validate requests and enforce business rules consistently.
- The desktop client can evolve without directly coupling to the database.
- Security-sensitive operations remain on the server rather than in the client.

## 3. Used JWT-based authentication

Users authenticate through the FastAPI backend and receive a signed JWT session
token. The client includes this token in the `Authorization` header for
subsequent API requests.

The token contains the user and church context, allowing the backend to scope
requests to the correct church.

## 4. Designed a multi-tenant data model

Vera is designed so that multiple churches can use the same system without
mixing their data.

Core records—including users, members, families, donations, payments, accounts,
transactions, events, and settings—are associated with a `church_id`. The backend
uses this context to scope queries and mutations.

## 5. Centralized financial workflows

Vera includes workflows for a chart of accounts, account transfers, income,
expenses, donations, and payment-method routing.

The goal was to reduce manual record-keeping and make financial activity easier
to organize and review. Payment-method routing allows administrators to define
which account should receive payments made through cash, cheque, card, or online
payment.

## 6. Added centralized error logging

Application errors are recorded in an `application_logs` table with context such
as the affected user, church, request method, URL, error message, and client IP.

This makes debugging easier and provides operational visibility when something
goes wrong.

## 7. Prioritized practical administrative workflows

The product was designed around real administrative needs: recording donations,
managing members and families, tracking accounts, processing payments, and
generating reports.

Rather than building every possible feature at once, the focus was on creating
clear workflows that help church administrators keep records organized and
consistent.

## Current status

Vera is an ongoing project. Current work focuses on refining financial and
reporting workflows while maintaining a clear separation between the desktop
client, backend API, and PostgreSQL database.
