# Create SQL Server DB & Tables using GitHub Actions
This repository contains a GitHub Actions workflow that automatically creates a SQL Server database and tables by executing SQL scripts using sqlcmd.
The workflow runs manually using workflow_dispatch and executes SQL files stored in the repository.

# Workflow Name
Create SQL Server DB & Table (From SQL File)

# Workflow Overview
The GitHub Action performs the following steps:

* Checkout Repository
* Install sqlcmd
* Create Database
* Create Tables
* Folder Structure

# Folder Structure
.
├── .github
│   └── workflows
│       └── create-db.yml
│
├── sql
│   ├── 01_create_database.sql
│   └── 02_create_tables.sql
│
└── README.md

# Required GitHub Secrets

You must configure the following Repository Secrets before running the workflow.

Go to:
GitHub Repository
→ Settings
→ Secrets and Variables
→ Actions
→ New Repository Secret

Add the following secrets:

| Secret Name | Description               |
| ----------- | ------------------------- |
| DB_SERVER   | SQL Server hostname or IP |
| DB_USER     | SQL Server username       |
| DB_PASSWORD | SQL Server password       |

SQL Files
# Create Database Script

File:
sql/01_create_database.sql

# Create Tables Script

File:
sql/02_create_tables.sql

# GitHub Action Workflow

Location:
.github/workflows/create-db.yml

Workflow Code:
name: Create SQL Server DB & Table (From SQL File)

on:
  workflow_dispatch:

jobs:
  create-table:
    runs-on: windows-latest

    steps:
      - name: Checkout repo
        uses: actions/checkout@v4

      - name: Install sqlcmd
        run: choco install sqlcmd -y

      # Step 1: Create Database
      - name: Create Database
        shell: pwsh
        run: |
          sqlcmd `
            -S "${{ secrets.DB_SERVER }}" `
            -U "${{ secrets.DB_USER }}" `
            -P "${{ secrets.DB_PASSWORD }}" `
            -d master `
            -i "sql/01_create_database.sql"

      # Step 2: Create Tables
      - name: Create Tables
        shell: pwsh
        run: |
          sqlcmd `
            -S "${{ secrets.DB_SERVER }}" `
            -U "${{ secrets.DB_USER }}" `
            -P "${{ secrets.DB_PASSWORD }}" `
            -d TestDB `
            -i "sql/02_create_tables.sql"

# How to Run the Workflow

Open your GitHub Repository
Click Actions
Select workflow:
Create SQL Server DB & Table (From SQL File)
Click Run workflow
Select branch
Click Run workflow

The workflow will:
* Connect to SQL Server
* Create the database
* Create tables

# Requirements

* SQL Server instance accessible from GitHub Actions
* Database credentials stored in GitHub Secrets
* SQL files placed in /sql folder

# Benefits of This Setup

✔ Automates database setup
✔ Works with CI/CD pipelines
✔ Keeps database scripts version controlled
✔ Easy to run manually
