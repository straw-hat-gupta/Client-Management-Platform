# Wealth Management Client Platform

A centralized platform for a wealth management firm to manage people from their first referral through onboarding, investment transfers, and ongoing client service.

The firm currently tracks this work across several Excel spreadsheets. This project will bring those workflows together so the team can see each person's status, next action, and history in one place.

## Project status

This project is in discovery and specification. Features will be defined before implementation using a spec-driven development process.

## Core workflow

1. **Referral Management System (RMS)** — Track new introductions and potential clients, including who referred them, contact details, meeting dates and notes, the decision to move forward, and the client number once created.
2. **New Client Consultancy Phase (NCCP)** — Guide new clients through the required wealth-management discovery and consultation work.
3. **Monitoring and Management (M&M)** — Manage ongoing client relationships after the consultancy phase, including follow-ups such as three-month thank-you cards and client appreciation events.

## Supporting pipelines

- **Pillar List** — Track people with current or potential investable assets, meeting status, and estimated investment amounts.
- **Focus 10** — Identify the ten people from the Pillar List most likely to bring new business within the next three months.
- **Investment Transfers** — Track signed investment transfers until the assets are received.
- **Insurance Business Monitor** — Track potential insurance business and its progress.
- **Transfer Reconciliation** — Reconcile received funds with their investment records.

## Goals

- Create one reliable view of prospects, new clients, and existing clients.
- Make ownership, status, and next actions clear.
- Reduce duplicate entry across spreadsheets.
- Prevent referrals, transfers, and follow-up tasks from being missed.
- Preserve an auditable history of client activity and decisions.

## Development approach

Each feature should move through the following process:

1. Document the business problem and current workflow.
2. Define requirements, terminology, data, rules, and acceptance criteria in a feature spec.
3. Review and approve the spec with stakeholders.
4. Implement only the approved scope.
5. Verify the implementation against the acceptance criteria.

Architecture, framework, and deployment decisions have intentionally not been made yet. They will be recorded once the product requirements and constraints are understood.

## Getting started

There is no runnable application yet. Begin by documenting and approving the first feature specification.
