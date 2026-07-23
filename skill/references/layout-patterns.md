# Layout Patterns for B2B Management Dashboards

## 1. Page Structure

### 1.1 Left Navigation + Main Content

The standard page layout:
- **Left sidebar** (width ~240px): Navigation menu with collapsible sections
- **Main content area** (fills remaining space): Dashboard, tables, or forms

The sidebar contains:
- **Brand header**: Logo + product name (fixed at top of sidebar)
- **Navigation sections**: Grouped by user workflow frequency, not by backend module
- **Nav items**: Icon + text, with active state indicator

### 1.2 Navigation Grouping Convention

Group navigation items by usage frequency, not by database entity:

```
概览 (Overview)
├── 数据看板 (Dashboard) ← default landing page

监控/业务模块 (Monitoring)
├── [Business data item 1]     ← high-frequency access
├── [Business data item 2]
└── [Business data item 3]

运营管理 (Operations)
├── [Resource CRUD item 1]    ← medium-frequency
├── [Resource CRUD item 2]
└── [Resource CRUD item 3]

系统 (System)
├── 异常提醒 (Alerts)
└── 系统设置 (Settings)
```

Low-frequency pages (reports, infrequent management tasks) should be hidden or
collapsed by default in v1. See `interaction-patterns.md` for the progressive
disclosure principle.

## 2. Dashboard Page

The main landing page after login. Contains three tiers.

### 2.1 Stat Cards Row

A horizontal row of 4-6 metric cards. Each card shows:
- Icon (Font Awesome icon)
- Metric value (large, bold)
- Metric label (smaller, muted text)
- Optional: trend indicator (up/down arrow with percentage change)

Cards use a white background with subtle shadow, no border-radius over 8px.

### 2.2 Hot / Critical Items Table

A compact table showing items that need attention:
- Rank / position number
- Item name
- Metric values (current vs previous periods)
- Trend indicator (color-coded up/down)
- Action button (view detail / trend chart)

### 2.3 Full Data Table

Below the hot items, a comprehensive data table with:
- Column headers with sort indicators
- Search/filter bar above the table
- Action buttons
- Pagination at bottom

## 3. Detail / Data Pages

Standard pattern for feature sub-pages:

```
┌─────────────────────────────────┐
│  Page Title                     │
├─────────────────────────────────┤
│  [Filter/Search Bar] [Button]   │
├─────────────────────────────────┤
│  Table rows...                 │
│                                 │
│  [Pagination]                  │
└─────────────────────────────────┘
```

Filter bar components in priority order:
1. Text search input
2. Category/filter dropdowns
3. Date range picker
4. Action buttons (Search, Reset)

Keep the filter bar compact — 38-40px height, uses the same spacing as the
table below it. Avoid stacking filters vertically when possible.

## 4. Form / Management Pages

For CRUD operations:
- **Inline forms**: Edit directly in the table row where possible
- **Modal dialogs**: For complex forms or bulk operations
- **Full page forms**: Only for setup/wizard flows

## 5. Responsive Behavior

At desktop widths (>1024px): sidebar visible, 4-6 stat cards in a row
At narrower widths (<1024px): collapsible sidebar, stat cards wrap to 2 per row

Use a single breakpoint at 920px for the prototype.
