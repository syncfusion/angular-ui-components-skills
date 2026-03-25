# Getting Started with Angular Gantt Chart

## Table of Contents
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [CSS Imports](#css-imports)
- [Add the Component](#add-the-component)
- [Bind Data](#bind-data)
- [Inject Feature Modules](#inject-feature-modules)
- [Configure Timeline](#configure-timeline)
- [Enable Toolbar](#enable-toolbar)
- [Enable Editing](#enable-editing)
- [Enable Filtering and Sorting](#enable-filtering-and-sorting)
- [Define Event Markers](#define-event-markers)
- [Handle Errors](#handle-errors)
- [Run the Application](#run-the-application)

---

## Prerequisites

- Angular 19+ (standalone architecture is the default)
- Node.js 18+
- Angular CLI installed globally

```bash
npm install -g @angular/cli
```

---

## Installation

Use `ng add` for automated setup (recommended). It installs the package, imports the module, and registers the default theme:

```bash
ng add @syncfusion/ej2-angular-gantt
```

Or install manually:

```bash
npm install @syncfusion/ej2-angular-gantt --save
```

**Dependencies automatically installed:**
- `@syncfusion/ej2-angular-base`
- `@syncfusion/ej2-gantt`
- `@syncfusion/ej2-grids`, `ej2-treegrid`, `ej2-data`
- `@syncfusion/ej2-calendars`, `ej2-dropdowns`, `ej2-inputs`, `ej2-buttons`, `ej2-popups`

---

## CSS Imports

Add theme styles to `styles.css`. Material3 is commonly used:

```css
@import '../node_modules/@syncfusion/ej2-base/styles/material3.css';
@import '../node_modules/@syncfusion/ej2-buttons/styles/material3.css';
@import '../node_modules/@syncfusion/ej2-calendars/styles/material3.css';
@import '../node_modules/@syncfusion/ej2-dropdowns/styles/material3.css';
@import '../node_modules/@syncfusion/ej2-inputs/styles/material3.css';
@import '../node_modules/@syncfusion/ej2-lists/styles/material3.css';
@import '../node_modules/@syncfusion/ej2-layouts/styles/material3.css';
@import '../node_modules/@syncfusion/ej2-navigations/styles/material3.css';
@import '../node_modules/@syncfusion/ej2-notifications/styles/material3.css';
@import '../node_modules/@syncfusion/ej2-popups/styles/material3.css';
@import '../node_modules/@syncfusion/ej2-splitbuttons/styles/material3.css';
@import '../node_modules/@syncfusion/ej2-grids/styles/material3.css';
@import '../node_modules/@syncfusion/ej2-treegrid/styles/material3.css';
@import '../node_modules/@syncfusion/ej2-gantt/styles/material3.css';
```

Import order matters — follow the dependency sequence above. Other available themes: `bootstrap5`, `fluent2`, `tailwind3`.

---

## Add the Component

Modify `src/app/app.ts` (Angular 20+) or `src/app/app.component.ts` (Angular 19 and below):

```typescript
import { Component, OnInit } from '@angular/core';
import { GanttModule } from '@syncfusion/ej2-angular-gantt';

@Component({
  imports: [GanttModule],
  standalone: true,
  selector: 'app-root',
  template: `<ejs-gantt></ejs-gantt>`
})
export class AppComponent implements OnInit {
  ngOnInit(): void {}
}
```

This renders an empty Gantt chart. Configure `dataSource` and `taskFields` to show tasks.

---

## Bind Data

The `dataSource` property accepts a JavaScript object array or `DataManager`. The `taskFields` property maps data fields to Gantt attributes:

```typescript
import { Component } from '@angular/core';
import { GanttModule } from '@syncfusion/ej2-angular-gantt';

@Component({
  imports: [GanttModule],
  standalone: true,
  selector: 'app-root',
  template: `
    <ejs-gantt
      [dataSource]="data"
      [taskFields]="taskFields"
      [height]="'450px'">
    </ejs-gantt>
  `
})
export class AppComponent {
  public data: object[] = [
    {
      TaskID: 1, TaskName: 'Project Initiation',
      StartDate: new Date('04/02/2024'), EndDate: new Date('04/21/2024'),
      subtasks: [
        { TaskID: 2, TaskName: 'Identify Site location', StartDate: new Date('04/02/2024'), Duration: 4, Progress: 50 },
        { TaskID: 3, TaskName: 'Perform Soil test', StartDate: new Date('04/02/2024'), Duration: 4, Progress: 50 },
        { TaskID: 4, TaskName: 'Soil test approval', StartDate: new Date('04/02/2024'), Duration: 4, Progress: 50, Predecessor: '3FS' }
      ]
    }
  ];
  public taskFields: object = {
    id: 'TaskID',
    name: 'TaskName',
    startDate: 'StartDate',
    endDate: 'EndDate',
    duration: 'Duration',
    progress: 'Progress',
    dependency: 'Predecessor',
    child: 'subtasks'
  };
}
```

**Essential `taskFields` mappings:**
| Property | Maps to | Required |
|----------|---------|----------|
| `id` | Unique task ID | Yes |
| `name` | Task name | Yes |
| `startDate` | Start date | Yes |
| `endDate` / `duration` | End date or duration | One required |
| `progress` | Completion % | Optional |
| `dependency` | Predecessor IDs | Optional |
| `child` | Child tasks array | For hierarchical data |
| `parentID` | Parent task ID | For self-referential data |

> **`dateFormat`:** Use the top-level `dateFormat` property to set the default display format for all date columns globally (e.g., `dateFormat="MM/dd/yyyy"`). Individual column `format` overrides this per-column.

---

## Inject Feature Modules

Inject services in the `providers` array to enable features:

```typescript
import { Component } from '@angular/core';
import {
  GanttModule,
  EditService, FilterService, SortService,
  ToolbarService, SelectionService, DayMarkersService
} from '@syncfusion/ej2-angular-gantt';

@Component({
  imports: [GanttModule],
  standalone: true,
  providers: [EditService, FilterService, SortService, ToolbarService, SelectionService, DayMarkersService],
  selector: 'app-root',
  template: `
    <ejs-gantt
      [dataSource]="data"
      [taskFields]="taskFields"
      [toolbar]="toolbar"
      [allowFiltering]="true"
      [allowSorting]="true"
      [editSettings]="editSettings">
    </ejs-gantt>
  `
})
export class AppComponent {
  public data: object[] = [];
  public taskFields: object = {};
  public toolbar: string[] = ['Add', 'Edit', 'Delete'];
  public editSettings: object = { allowEditing: true, allowAdding: true, allowDeleting: true };
}
```

Omitting a required service disables its feature at runtime.

---

## Configure Timeline

Use `timelineSettings` to define the top/bottom tiers and date formats:

```typescript
public timelineSettings: object = {
  topTier: {
    unit: 'Week',
    format: 'MMM dd, yyyy'
  },
  bottomTier: {
    unit: 'Day',
    count: 1
  }
};
```

Set project date range:
```html
<ejs-gantt
  [timelineSettings]="timelineSettings"
  projectStartDate="04/01/2024"
  projectEndDate="06/30/2024">
</ejs-gantt>
```

---

## Enable Toolbar

Add `ToolbarService` to providers and set the `toolbar` array:

```typescript
public toolbar: string[] = ['Add', 'Edit', 'Delete', 'ExpandAll', 'CollapseAll', 'Search', 'ZoomIn', 'ZoomOut', 'ZoomToFit'];
```

Without `ToolbarService`, toolbar won't render.

---

## Enable Editing

Set `editSettings` and inject `EditService`:

```typescript
public editSettings: object = {
  allowEditing: true,      // Enable cell/dialog editing
  allowAdding: true,       // Enable adding new tasks
  allowDeleting: true,     // Enable deleting tasks
  allowTaskbarEditing: true, // Enable drag/resize taskbars
  mode: 'Auto'             // 'Auto' = cell editing | 'Dialog' = dialog editing
};
```

- **Cell editing:** `mode: 'Auto'` — double-click a TreeGrid cell
- **Dialog editing:** `mode: 'Dialog'` — double-click anywhere opens dialog
- **Taskbar editing:** `allowTaskbarEditing: true` — drag/resize taskbars on the chart

---

## Enable Filtering and Sorting

```html
<ejs-gantt [allowFiltering]="true" [allowSorting]="true">
</ejs-gantt>
```

Inject `FilterService` and `SortService` in providers. Filtering adds filter icons to column headers; sorting enables click-to-sort.

---

## Define Event Markers

Event markers highlight important project dates with vertical lines:

```typescript
// Inject DayMarkersService
public eventMarkers: object[] = [
  { day: new Date('04/10/2024'), label: 'Kickoff meeting', cssClass: 'e-custom-event-marker' },
  { day: new Date('05/15/2024'), label: 'Mid-project review' }
];
```

```html
<ejs-gantt [eventMarkers]="eventMarkers"></ejs-gantt>
```

Missing `DayMarkersService` prevents marker rendering.

---

## Handle Errors

Subscribe to the `actionFailure` event to catch configuration issues:

```typescript
public actionFailure(args: any): void {
  console.error('Gantt action failed:', args.error);
}
```

```html
<ejs-gantt (actionFailure)="actionFailure($event)"></ejs-gantt>
```

Common causes: missing `isPrimaryKey`, invalid `dependency` format, missing `hasChildMapping` for load-on-demand, invalid `timelineSettings.format`.


---

## Grid Lines

Control which grid lines are rendered in the TreeGrid (left) and chart (right) panes:

```html
<ejs-gantt gridLines="Both"></ejs-gantt>
```

| Value | Description |
|---|---|
| `'Both'` | Horizontal and vertical lines in both panes (default) |
| `'Horizontal'` | Only horizontal row separator lines |
| `'Vertical'` | Only vertical column divider lines |
| `'None'` | No grid lines |

```typescript
// Programmatic toggle
this.ganttObj.gridLines = 'Horizontal';
```

---

## Run the Application

```bash
ng serve --open
```

The browser opens at `http://localhost:4200` showing the Gantt chart.

**Troubleshooting checklist:**
- Styles not showing → Verify CSS import order in `styles.css`
- Editing not working → Check `EditService` is in `providers`
- Toolbar not visible → Check `ToolbarService` is in `providers`
- Tasks not rendering → Verify `taskFields.id`, `taskFields.name`, `taskFields.startDate` are mapped
