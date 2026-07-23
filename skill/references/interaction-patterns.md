# Interaction Design Principles for B2B Dashboards

These principles were discovered through real project iteration. Follow them
when building a new dashboard and during iterative refinement.

## 1. Data Density First

**Principle**: Prioritize the dashboard view (stat cards + critical items table)
over full feature coverage. In v1, invest 80% of effort in the pages users will
visit daily and 20% in the rest.

**Why**: B2B users spend most of their time on the landing dashboard. A dense,
information-rich dashboard is more valuable than having every sub-page fully
implemented.

**How**: Before writing code, rank all user stories by frequency of use. Build
the top 2-3 feature areas first. Everything else can be a placeholder or hidden
in v1.

## 2. Progressive Disclosure

**Principle**: Hide low-frequency pages in v1. Comment them out in the
navigation rather than removing the code. This makes them easy to restore when
needed without rebuilding.

**Why**: Fewer visible options reduces cognitive load for the user. Hidden
functionality can be revealed when the user is ready without destroying their
existing workflow.

**How**: In the sidebar nav, use HTML comments to mark hidden items:

```html
<!-- 低频率功能 - v2 时取消注释
<div class="nav-item" data-page="reports">
  <i class="fas fa-file-alt"></i><span>数据报表</span>
</div>
-->
```

## 3. Filter-First, CRUD-Second

**Principle**: Add search/filter capabilities to data tables before implementing
full CRUD (Create, Read, Update, Delete) forms.

**Why**: Users need to find existing data before they can manage it. Filters
provide immediate value and are cheaper to implement than full CRUD flows.

**How**: Each data table page should get, in this order:
1. Text search input (v0.5)
2. Category/filter dropdowns + date picker (v0.5)
3. Pagination (v1)
4. Inline editing (v2)
5. Bulk actions (v3)

## 4. Realistic Mock Data Early

**Principle**: Use realistic, domain-appropriate mock data from the start.
Avoid placeholder text like "xxx" or "待填".

**Why**: Realistic data helps the user (and stakeholders) evaluate whether the
dashboard actually serves their needs. It also catches layout problems that only
appear with real-length strings and real numeric magnitudes.

**How**: Generate mock data that matches the domain:
- For travel: use real city names, hotel names, price ranges
- For finance: use realistic amounts, dates, account numbers
- For logistics: use real locations, tracking numbers, delivery statuses

## 5. Navigate by Frequency, Not by Module

**Principle**: Organize sidebar navigation by how often users access each page,
not by the backend modules they correspond to.

**Why**: Pure backend module grouping creates mixed-frequency sections. For
example, "运营管理" should not be at the bottom if the user visits that section
10 times a day.

**How**: Group items into tiers:
- 概览 (always first — highest frequency)
- 业务/监控模块 (medium-to-high frequency, grouped by workflow)
- 运营管理 (medium frequency)
- 系统 (lowest frequency — alerts, settings, admin)
- Hidden items (for future releases)

## 6. Single File Prototype Strategy

**Principle**: Keep the prototype as a single self-contained HTML file until
there is a clear need for a framework or build step.

**Why**: Single HTML files have zero setup cost, can be opened in any browser,
and make iteration feedback loops nearly instant. Switching to a framework too
early slows down exploration.

**How**: Use the seed template from `assets/template.html` as the starting
point. Add new pages as sections within the same file. Don't introduce
 React/Vue/Svelte or a build pipeline until the prototype is validated.
 
 ## 7. Quick Filter Button Groups
 
 **Principle**: Add compact button groups for common filter values (rank ranges,
 time periods, status categories) alongside standard dropdown filters.
 
 **Why**: Button groups let users apply frequent filters with one click instead
 of opening a dropdown and scanning options. In practice, users reach for quick
 filters far more often than custom filter combinations.
 
 **How**: Use a group of small buttons with `.active` toggle state. Place them
 in the table-header-left or filter-bar area. Typical examples:
 - Rank ranges: 全部 / 前5 / 前50 / 未出现
 - Time periods: 7天 / 14天 / 30天
 - Quick toggles in chart cards to switch aggregation windows
 
 ```html
 <div class="rank-filter-group">
   <button class="rank-filter-btn active" onclick="filterList('all')">全部</button>
   <button class="rank-filter-btn" onclick="filterList('top5')">前5</button>
   <button class="rank-filter-btn" onclick="filterList('top50')">前50</button>
   <button class="rank-filter-btn" onclick="filterList('notAppeared')">未出现</button>
 </div>
 ```
 
 CSS: see `template.html` — `.rank-filter-group` / `.rank-filter-btn` styles.
 
 ## 8. Stats Summary Bar Above Management Pages
 
 **Principle**: Place a compact stats summary row above the filter bar on CRUD
 / management pages, showing aggregate counts with color-coded status dots.
 
 **Why**: Users need to see the overall picture before diving into a list.
 Aggregate counts (total records, active, pending, eliminated) give immediate
 context without scrolling or counting. Colored dots let users visually parse
 status distribution at a glance.
 
 **How**: Structure as a horizontal row of `kw-stat-item` entries, each with a
 colored dot (`.stat-dot`), label, and number. Place it as the first child of
 the `table-card`, above the filter bar.
 
 ```html
 <div class="kw-stats">
   <span class="kw-stat-item"><strong>总记录</strong> <span id="statTotal">0</span></span>
   <span class="kw-stat-item"><span class="stat-dot active"></span> 正常 <span id="statActive">0</span></span>
   <span class="kw-stat-item"><span class="stat-dot warning"></span> 待处理 <span id="statPending">0</span></span>
   <span class="kw-stat-item"><span class="stat-dot inactive"></span> 已关闭 <span id="statClosed">0</span></span>
 </div>
 ```
 
 CSS: see `template.html` — `.kw-stats` / `.kw-stat-item` / `.stat-dot` styles.
 
 ## 9. Modal Data Capabilities
 
 **Principle**: When a modal displays data (trend chart, detail list), it
 should include time range filters and an export button. Do not show static
 snapshots.
 
 **Why**: A modal showing data is a mini analysis interface. Users naturally
 want to narrow the time window or export the data -- these capabilities cost
 little to add in v1 but are frustrating to add retroactively.
 
 **How**: Add a filter row inside the modal header or body for date range
 inputs, and a primary button for export (CSV/image). Wire the chart and list
 rendering to the filter values so they update together.
 
 ```html
 <div class="modal">
   <div class="modal-header">
     <span class="modal-title" id="trendModalTitle">趋势 - xxx</span>
     <button class="modal-close" onclick="closeModal()"><i class="fas fa-times"></i></button>
   </div>
   <div class="modal-body">
     <div class="trend-date-range" style="display:flex;gap:8px;align-items:center;margin-bottom:14px;">
       <label class="trend-label" style="font-size:13px;color:var(--text-muted);">时间范围</label>
       <input class="form-input date-input" type="date" id="trendDateStart" style="width:150px;">
       <span style="color:var(--text-muted);">至</span>
       <input class="form-input date-input" type="date" id="trendDateEnd" style="width:150px;">
       <button class="btn btn-primary btn-sm" onclick="exportTrendData()"><i class="fas fa-download"></i> 导出</button>
     </div>
     <div class="trend-chart-container" style="height:240px;margin-bottom:16px;"><canvas id="trendChart"></canvas></div>
     <div class="trend-list-wrap" style="max-height:260px;overflow-y:auto;border:1px solid var(--border-color);border-radius:6px;">
       <table class="data-table"><thead><tr><th>日期</th><th>排名</th></tr></thead><tbody id="trendListBody"></tbody></table>
     </div>
   </div>
 </div>
 ```
 
 ## 10. Filter Select vs Form Select Distinction
 
 **Principle**: Distinguish between `.filter-select` (compact, inline in toolbar,
 height ~33px, text vertically centered via `line-height:33px`) and `.form-select`
 (taller ~36px, in modal/panel forms with `padding:8px 28px 8px 12px`, proper
 `line-height:1.5`). Do not reuse one for both contexts.
 
 **Why**: Inline filter selects need to sit at the same height as search inputs
 and buttons in a horizontal toolbar. Form selects inside modals need taller
 padding to match other form inputs. Using one style for both produces either
 cramped forms or overly tall toolbars.
 
 **How**: See `template.html` where both classes are defined separately with
 comments explaining their usage context.
 
 ## 11. Data Model Evolution Expectation
 
 **Principle**: B2B data models frequently evolve from flat lists to
 one-to-many or many-to-many relationships. Design data structures and UI
 rendering to accommodate this evolution.
 
 **Why**: A simple keyword list later needs allocation records (one keyword
 to many account assignments). A flat item list later needs tags, history, or
 hierarchy. When the data layer uses arrays + relational IDs and the UI uses
 merged cells (rowspan) instead of row-per-record, switching to a relational
 model is a data change, not a rewrite.
 
 **How**: 
 - Store data as arrays of objects with explicit `id` and foreign-key fields
 - In tables, use `rowspan` on the parent key cell to show one-to-many visually
 - Keep rendering functions generic: iterate assignments, not rows
 - Add a stats summary bar (see pattern #8) above the table to compensate for
   increased visual complexity
