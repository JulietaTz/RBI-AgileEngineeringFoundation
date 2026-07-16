# Workflow 01: Test Context Analysis

## Purpose

Create a shared and reviewable understanding of the test assignment before
planning or test design starts.

## Trigger

- new project,
- new release,
- major scope change,
- missing test context.

## Responsible Agent

- test-orchestrator

## Required Inputs

- product description or user request
- available requirements
- known stakeholders
- known constraints

## Steps

1. Inventory available input documents.
2. Create a source list.
3. Identify product and business goal.
4. Define preliminary scope.
5. Identify stakeholders and approval roles.
6. Identify constraints and dependencies.
7. Record known quality risks.
8. Record missing information.
9. Create the test context artifact.
10. Run Quality Gate QG-01.
11. Create handover to Test Manager or Test Analyst.

## Output

- projects/<project>/working/test-context.md
- projects/<project>/working/handovers/context-handover.md

## Stop Conditions

Stop the workflow when:

- the product under test cannot be identified,
- no test assignment exists,
- scope cannot be distinguished from assumptions,
- required legal or security constraints are unknown.