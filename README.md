# HR-Payroll Data Synchronization Platform

A Python-based prototype that demonstrates how employee data changes can be synchronized between HR and Payroll systems using Model Context Protocol (MCP) and LangChain.

## Overview

Organizations often maintain employee information across multiple systems. When employee details such as salary, department, or designation are updated in the HR system, those changes must also be reflected in Payroll.

This project simulates that workflow by detecting employee changes, validating updates, generating synchronization payloads, and applying approved changes to a Payroll database.

## Key Features

* Detect employee record changes automatically
* Capture updates using SQLite database triggers
* Validate employee updates before synchronization
* Generate structured JSON sync payloads
* Transfer updates between HR and Payroll systems
* Apply approved changes to the Payroll database
* Expose functionality through MCP tools

## Architecture

HR Database
→ Change Detection
→ Validation
→ Payload Generation
→ Payroll Validation
→ Payroll Update

## Workflow

1. Employee information is updated in the HR database.
2. SQLite triggers record the change in a change log table.
3. The HR agent detects unprocessed changes.
4. Validation rules are applied.
5. A synchronization payload is generated.
6. The Payroll agent receives and validates the payload.
7. Changes are applied to the Payroll database.

## MCP Tools

### HR MCP Server

* detect_changes
* validate_changes
* generate_payload

### Payroll MCP Server

* receive_payload
* validate_sync_data
* apply_changes

## Example Use Case

A salary update is made in the HR system.

Before:

Employee ID: 1001
Salary: 95,000

After:

Employee ID: 1001
Salary: 98,000

The change is detected, validated, transferred, and applied to the Payroll database automatically.

## Tech Stack

* Python
* LangChain
* MCP (Model Context Protocol)
* SQLite
* JSON

## Learning Outcomes

This project helped me understand:

* MCP server development
* Tool-based system integration
* Workflow automation
* Change data capture concepts
* Multi-step business process implementation
* Agent-driven task execution
