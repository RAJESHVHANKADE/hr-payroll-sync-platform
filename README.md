# HR-Payroll Data Synchronization Platform

A prototype integration platform that demonstrates how employee data changes can be automatically synchronized between HR and Payroll systems using Model Context Protocol (MCP), LangChain, and Python.

The project simulates a common enterprise workflow where employee records maintained in an HR system must remain consistent with records stored in a Payroll system. It detects employee updates, validates business rules, generates synchronization payloads, and applies approved changes to the target system.

---

## Business Problem

Organizations often maintain employee information across multiple applications. When changes such as salary revisions, department transfers, designation updates, or employee status changes occur in the HR system, those updates must also be reflected in Payroll.

Manual synchronization can lead to:

* Data inconsistencies between systems
* Delayed payroll processing
* Increased operational effort
* Human errors during data updates

This project demonstrates an automated approach to detecting and synchronizing employee changes between independent systems.

---

## Solution Overview

The platform consists of two independent systems:

### HR System

Responsible for:

* Managing employee records
* Detecting employee data changes
* Validating updates
* Generating synchronization payloads

### Payroll System

Responsible for:

* Receiving employee updates
* Validating incoming data
* Applying approved changes
* Maintaining synchronized payroll records

Communication between systems is implemented through MCP tools and structured JSON payloads.

---

## Architecture

```text
┌─────────────────────┐
│      HR System      │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│ SQLite Triggers     │
│ Change Detection    │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│ change_log Table    │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│ HR MCP Server       │
│                     │
│ • detect_changes    │
│ • validate_changes  │
│ • generate_payload  │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│ HR Agent            │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│ JSON Sync Payload   │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│ Payroll Agent       │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│ Payroll MCP Server  │
│                     │
│ • receive_payload   │
│ • validate_sync     │
│ • apply_changes     │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│ Payroll Database    │
└─────────────────────┘
```

---

## Key Features

### Change Detection

Implemented SQLite triggers to automatically capture employee record updates and store them in a dedicated change log table.

Captured events include:

* Employee salary updates
* Department changes
* Designation updates
* Employee information modifications

---

### Validation Layer

Before synchronization, employee updates are validated against business rules to prevent invalid data from reaching the Payroll system.

Example validations:

* Employee ID must exist
* Salary values must be valid
* Required employee fields cannot be empty
* Duplicate change records are ignored

---

### Payload Generation

Validated employee updates are transformed into structured JSON payloads containing:

* Employee information
* Change type
* Updated values
* Synchronization metadata
* Processing timestamps

---

### Payroll Synchronization

The Payroll system receives incoming payloads, performs validation checks, and applies approved updates to maintain data consistency.

---

### MCP Tool Integration

The project exposes business functionality through MCP tools.

#### HR MCP Server

| Tool             | Purpose                           |
| ---------------- | --------------------------------- |
| detect_changes   | Retrieve pending employee updates |
| validate_changes | Validate employee records         |
| generate_payload | Create synchronization payload    |

#### Payroll MCP Server

| Tool               | Purpose                  |
| ------------------ | ------------------------ |
| receive_payload    | Receive incoming updates |
| validate_sync_data | Validate payload data    |
| apply_changes      | Update Payroll database  |

---

## Example Workflow

### Scenario

An HR administrator updates an employee salary.

Before Update

```json
{
  "employee_id": 1001,
  "name": "John Doe",
  "salary": 95000
}
```

After Update

```json
{
  "employee_id": 1001,
  "name": "John Doe",
  "salary": 98000
}
```

### Processing Flow

1. Salary update occurs in HR database.
2. SQLite trigger records the change.
3. HR Agent detects unprocessed updates.
4. Validation checks are executed.
5. JSON synchronization payload is generated.
6. Payroll Agent receives payload.
7. Payroll validation is performed.
8. Payroll database is updated.
9. Synchronization status is recorded.

Result:

The employee's payroll record now reflects the updated salary.

---

## Project Structure

```text
hr-payroll-sync-platform/

├── data/
│   ├── hr_system.db
│   ├── payroll_system.db
│   └── sync_payload.json
│
├── scripts/
│   ├── init_hr_db.py
│   ├── init_payroll_db.py
│   └── test_changes.py
│
├── hr_mcp_server.py
├── payroll_mcp_server.py
├── hr_agent.py
├── payroll_agent.py
│
├── requirements.txt
├── .env.example
├── README.md
└── .gitignore
```

---

## Technology Stack

* Python
* LangChain
* Model Context Protocol (MCP)
* SQLite
* JSON
* Git

---

## Technical Concepts Demonstrated

* MCP Server Development
* Tool-Based System Integration
* Workflow Automation
* Change Data Capture (CDC)
* Database Triggers
* Agent-Based Task Execution
* Data Validation Pipelines
* Cross-System Data Synchronization
* Structured Payload Generation

---

## Learning Outcomes

Through this project, I gained practical experience with:

* Designing MCP servers and tools
* Building workflow-driven applications
* Implementing change detection using database triggers
* Creating validation and synchronization pipelines
* Simulating enterprise system integration scenarios
* Managing data consistency across independent systems

```
```
