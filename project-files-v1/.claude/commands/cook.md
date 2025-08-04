---
description: "Orchestrate the complete development workflow: automatically pick up next task, create branch, generate ERD, and set up implementation"
tools: [ "Task", "TodoWrite" ]
---

# Cook Command - Complete Development Workflow

You are the orchestrator for a complete development workflow. Execute these steps in sequence:

## Step 1: Analyze Jira Task & Plan Review

Use the **jira-task-analyzer** agent to:

- Automatically find the next task to work on (highest priority "Open" or "Planning" status assigned to current user)
- If $ARGUMENTS is provided, use that specific task ID instead
- Analyze requirements and acceptance criteria
- Create implementation plan at `docs/{task-id}/plan.md`

Use the **plan-reviewer** agent to:

- Review the implementation plan for completeness and accuracy
- Compare against original Jira requirements
- Validate technical approaches and A2Z Sync patterns
- Generate approval status for plan quality

**If plan review status is NOT ✅ Plan Ready:**

- Pass the review feedback to **jira-task-analyzer** agent
- Have jira-task-analyzer address the specific plan issues raised
- Repeat plan review until approved
- Continue iteration until plan review status is ✅ Plan Ready

## Step 2: Create Git Branch

Use the **git-branch-manager** agent to:

- Create appropriately named branch following A2Z Sync conventions
- Checkout the new branch
- Set upstream tracking

## Step 3: Generate Engineering Requirements Document

Use the **code-architect** agent to:

- Review the implementation plan created in Step 1
- Analyze existing codebase architecture and patterns
- Create detailed ERD at `docs/{task-id}/erd.md`
- Update TodoWrite with refined technical tasks

## Step 4: Implement Code

Use the **code-implementer** agent to:

- Read the ERD and current TodoWrite list
- Implement code following technical specifications exactly
- Create new classes, tests, and modify existing files
- Update TodoWrite marking tasks completed as they're implemented
- Follow A2Z Sync patterns and Laravel best practices

## Step 5: Run Tests

Use the **test-runner** agent to:

- Execute relevant test suites based on implemented changes
- Run unit, integration, and regression tests
- Analyze test results and identify any failures
- Provide specific feedback on issues that need fixing

## Step 6: Code Review & Iteration

Use the **code-reviewer** agent to:

- Review implemented code for quality and standards compliance
- Check security, performance, and maintainability
- Validate A2Z Sync patterns and Laravel best practices
- Generate approval status for commit readiness

**If code review status is NOT ✅ Ready to Commit:**

- Pass the review feedback to **code-implementer** agent
- Have code-implementer address the specific issues raised
- Repeat Step 5 (test-runner) and Step 6 (code-reviewer) until approved
- Continue iteration until code review status is ✅ Ready to Commit

## Step 7: Create Commit

Use the **commit-manager** agent to:

- Analyze changed files and completed work
- Generate properly formatted commit message with task ID
- Stage appropriate files for commit
- Execute commit following A2Z Sync conventions

## Step 8: Summary Report

Provide a concise summary including:

- ✅ Task analyzed and plan created
- ✅ Branch created and checked out
- ✅ ERD generated with technical specifications
- ✅ Code implemented following ERD specifications
- ✅ Tests executed and validated
- ✅ Code reviewed and approved
- ✅ Changes committed with proper formatting
- 📋 TodoWrite progress (completed vs remaining tasks)
- 🎯 Ready for push and pull request

## Guidelines:

- Wait for each agent to complete before proceeding to the next step
- After each agent completes, give the user a quick update, and continue
- If any step fails, stop and report the issue
- Each agent should report back what they accomplished
- Maintain the todo list throughout the process
- Focus on A2Z Sync's Laravel multi-tenant architecture

Execute this workflow for: **${ARGUMENTS:-"next available task"}**