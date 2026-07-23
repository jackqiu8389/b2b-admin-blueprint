---
name: b2b-dashboard
description: |
  General-purpose B2B management dashboard skill. Covers the full workflow from 
  PRD writing to frontend implementation for operational/admin panels. Use when 
  building any internal operation dashboards, management backends, admin panels, 
  or monitoring tools. Triggers include: "创建管理后台" "运营后台" "后台系统" 
  "管理面板" "内部系统" "dashboard" "B2B backend" "admin panel" 
  "internal ops tool" "management dashboard".
---

# B2B Dashboard

## Overview

This skill provides a structured workflow for building B2B management dashboards.
It separates concerns into four layers: requirements (PRD), layout patterns,
interaction design principles, and a reusable HTML seed template.

## Workflow

When the user requests a new management dashboard, follow these steps:

### 1. Requirements — PRD

Read `references/prd-template.md`. Use it to structure the conversation with the
user and produce a PRD before writing any code. The PRD should cover:
- Problem Statement (why this is needed)
- Solution (what to build)
- User Stories grouped by feature area

### 2. Layout — Page Structure

Read `references/layout-patterns.md`. Identify which sections apply to this 
project. Key decisions:
- Choose the navigation structure (left-side collapsible sidebar)
- Pick stat cards for the dashboard landing page
- Design detail pages with filter bars and data tables
- Map user stories to page sections

### 3. Implementation — Seed Template

Use `assets/template.html` as the starting HTML. Before editing, replace:
- `BRAND_LOGO_URL` with the actual logo URL
- `BRAND_NAME` and `BRAND_SUBTITLE` with the actual brand identity
- CSS variables in `:root` with the brand colors
- Navigation items with the actual pages
- Stat cards and tables with actual data patterns

### 4. Iteration — Interaction Principles

Read `references/interaction-patterns.md` during iteration. These principles
were discovered through real project experience and guide feature priority,
information density decisions, and mock data strategy. Pay special attention
to patterns #7-#11 (Quick Filter Buttons, Stats Bar, Modal Data Capabilities,
Filter vs Form Select Distinction, Data Model Evolution) -- they come from
real project iteration and help anticipate common refactoring directions.

## Resources

### references/
- `prd-template.md` — PRD writing structure template
- `layout-patterns.md` — Page layout patterns (nav layout, cards, tables, filters)
- `interaction-patterns.md` — Design principles from iteration experience

### assets/
- `template.html` — Seed HTML template with purple accent default palette + CSS variable architecture. Contains: sidebar nav, stat cards row, hot items table, detail page with filter bar. Customize CSS variables and navigation items before use.
