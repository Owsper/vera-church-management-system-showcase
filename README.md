# Vera — Church Management System

Vera is a private client–server church management application designed to help
church administrators manage members, families, donations, payments, finances,
expenses, income, events, reports, and settings.

This repository is a public project showcase. The application source code is
private because Vera is an active product.

## Problem

Church offices often manage member records, donations, accounting, expenses,
events, and reports through separate files or manual processes. Vera centralizes
these workflows in one system with church-scoped data access.

## My role

I independently managed the product end to end, including:

- Requirements gathering
- User workflow and UI/UX design
- API design
- PostgreSQL database modeling
- Backend and desktop-client development
- Testing and iterative improvement

## Key features

- Member and family management
- Donation and payment recording
- Chart-of-accounts and financial workflows
- Account transfers
- Income and expense tracking
- Payment-method routing
- Event and attendee management
- Dashboard and report generation/export
- JWT-based authentication
- Multi-tenant, church-scoped data access
- Centralized application error logging

## Architecture

```text
PySide6 Desktop Client
        |
        | HTTP / JSON + Bearer JWT
        v
FastAPI Backend
        |
        | psycopg
        v
PostgreSQL Database
```

## Tech stack

- Python
- PySide6
- FastAPI
- PostgreSQL
- psycopg
- PyJWT / python-jose
- bcrypt
- Pandas
- ReportLab
- OpenPyXL

## Product decisions

### Moved from SQLite to PostgreSQL

The initial version used a local SQLite database. As Vera evolved into a
client–server system, PostgreSQL was selected for centralized storage, stronger
relational integrity, and a more scalable foundation for financial records.

### Separated UI from backend

The PySide6 client communicates with a FastAPI backend rather than accessing the
database directly. This separates the user interface, business logic, and data
storage.

### Scoped data by church

JWT sessions include a church_id, and key records are scoped by that ID. This
prevents data from being mixed between churches.

## Screenshots

Screenshots use demonstration data only. No real member, donation, or financial
data is shown.

| Dashboard | Members |
|---|---|
| ![Dashboard](docs/screenshots/dashboard.png) | ![Members](docs/screenshots/members.png) |

## Status

Vera is under active development. Current work focuses on refining financial and
reporting workflows.