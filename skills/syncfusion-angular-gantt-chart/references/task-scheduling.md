# Task Scheduling in Angular Gantt Chart

## Table of Contents
- [Scheduling Modes Overview](#scheduling-modes-overview)
- [Auto Scheduling](#auto-scheduling)
- [Manual Scheduling](#manual-scheduling)
- [Custom (Mixed) Scheduling](#custom-mixed-scheduling)
- [Unscheduled Tasks](#unscheduled-tasks)
- [Duration Units](#duration-units)
- [Working Days and Hours](#working-days-and-hours)
- [Baseline Dates](#baseline-dates)
- [Work Scheduling (Effort-Driven)](#work-scheduling-effort-driven)

---

## Scheduling Modes Overview

The `taskMode` property controls how task dates are calculated:

| Mode | `taskMode` value | Behavior |
|------|-----------------|----------|
| **Auto** | `'Auto'` (default) | Dates auto-calculated based on dependencies, holidays, and working time |
| **Manual** | `'Manual'` | Dates remain exactly as specified in the data source |
| **Custom** | `'Custom'` | Per-task mode determined by a boolean field in the data source |

---

## Auto Scheduling

Default behavior. Task start and end dates are automatically validated and adjusted based on:
- Working hours and non-working time
- Holidays and weekends
- Predecessor task completion

Parent task dates are derived from the min/max dates of child tasks — you cannot directly edit parent task start/end or progress in auto mode.

```typescript
@Component({
  template: `<ejs-gantt [dataSource]="data" [taskFields]="taskFields" taskMode="Auto"></ejs-gantt>`
})
export class AppComponent {
  public taskFields: object = {
    id: 'TaskID', name: 'TaskName',
    startDate: 'StartDate', duration: 'Duration',
    progress: 'Progress', dependency: 'Predecessor', child: 'subtasks'
  };
}
```

Use auto scheduling when you want the Gantt to enforce project logic automatically.

---

## Manual Scheduling

All task dates are fixed as entered in the data source. The component does not adjust dates based on dependencies or calendars.

```typescript
@Component({
  template: `<ejs-gantt [dataSource]="data" [taskFields]="taskFields" taskMode="Manual"></ejs-gantt>`
})
```

To still validate predecessors while using manual mode, enable:
```html
<ejs-gantt taskMode="Manual" [validateManualTasksOnLinking]="true"></ejs-gantt>
```

This automatically adjusts task dates when dependency links are created or modified, while keeping other dates fixed.

---

## Custom (Mixed) Scheduling

Individual tasks can have different scheduling modes using a boolean field in the data source:

```typescript
public data: object[] = [
  { TaskID: 1, TaskName: 'Auto task', StartDate: new Date('04/02/2024'), Duration: 3, isManual: false },
  { TaskID: 2, TaskName: 'Manual task', StartDate: new Date('04/02/2024'), Duration: 5, isManual: true }
];

public taskFields: object = {
  id: 'TaskID',
  name: 'TaskName',
  startDate: 'StartDate',
  duration: 'Duration',
  manual: 'isManual'   // Maps per-task scheduling mode
};
```

```html
<ejs-gantt [dataSource]="data" [taskFields]="taskFields" taskMode="Custom"></ejs-gantt>
```

`isManual: true` → task uses manual mode; `isManual: false` → task uses auto mode.

---

## Unscheduled Tasks

Tasks without complete scheduling information (missing start date, end date, or duration) can be rendered using `allowUnscheduledTasks`:

```html
<ejs-gantt [allowUnscheduledTasks]="true"></ejs-gantt>
```

Supported unscheduled states:
- **Start date only** — rendered as a milestone or open-ended bar
- **End date only** — bar starts at project beginning
- **Duration only** — floating bar with no fixed position
- **No dates at all** — task shown without positioned taskbar

This is useful for backlog items or placeholder tasks that have not been scheduled yet.

---

## Duration Units

The default duration unit is **days**. Configure using `durationUnit`:

```html
<ejs-gantt durationUnit="Hour"></ejs-gantt>
```

Available units: `Day` | `Hour` | `Minute`

Map the unit per task using `taskFields.durationUnit` if individual tasks use different units:

```typescript
public taskFields: object = {
  id: 'TaskID', name: 'TaskName',
  startDate: 'StartDate', duration: 'Duration',
  durationUnit: 'DurationUnit'  // Field in data source with values: 'day', 'hour', 'minute'
};
```

---

## Working Days and Hours

### Work Week

Configure which days of the week are working days using `workWeek`. The default is Monday through Friday:

```html
<ejs-gantt [workWeek]="['Monday','Tuesday','Wednesday','Thursday','Friday']">
</ejs-gantt>
```

Six-day work week (Monday–Saturday):
```html
<ejs-gantt [workWeek]="['Monday','Tuesday','Wednesday','Thursday','Friday','Saturday']">
</ejs-gantt>
```

Valid values: `'Sunday'`, `'Monday'`, `'Tuesday'`, `'Wednesday'`, `'Thursday'`, `'Friday'`, `'Saturday'`

> **Effect:** Non-working days are excluded from duration calculations. A 5-day task that starts on Thursday in a Mon–Fri week will end the following Wednesday, skipping Saturday and Sunday.

### Include Weekends

To make every day a working day (including weekends), use `includeWeekend`:

```html
<ejs-gantt [includeWeekend]="true"></ejs-gantt>
```

When `true`, all 7 days are treated as working days regardless of `workWeek`. Default is `false`.

### Daily Working Hours

Configure the working hours within each working day using `dayWorkingTime`. This affects duration calculations when `durationUnit: 'Hour'` or work-based scheduling is used:

```typescript
public dayWorkingTime: object[] = [
  { from: 8, to: 12 },   // 8 AM – 12 PM
  { from: 13, to: 17 }   // 1 PM – 5 PM (excludes 1-hour lunch break)
];
```

```html
<ejs-gantt [dayWorkingTime]="dayWorkingTime"></ejs-gantt>
```

Default is a single block `[{ from: 8, to: 17 }]` (8 AM to 5 PM = 9 working hours/day).

Multiple ranges are supported for split shifts or flexible schedules.

| Property | Working hours/day | Example |
|---|---|---|
| `[{ from: 8, to: 17 }]` | 9 hrs | Standard office hours |
| `[{ from: 8, to: 12 }, { from: 13, to: 17 }]` | 8 hrs | With lunch break |
| `[{ from: 6, to: 14 }]` | 8 hrs | Morning shift |
| `[{ from: 0, to: 24 }]` | 24 hrs | Round-the-clock |

---

## Baseline Dates

Baseline shows the originally planned dates alongside current dates for comparison:

```typescript
public taskFields: object = {
  id: 'TaskID',
  name: 'TaskName',
  startDate: 'StartDate',
  duration: 'Duration',
  baselineStartDate: 'BaselineStartDate',  // Original planned start
  baselineEndDate: 'BaselineEndDate'       // Original planned end
};
```

```html
<ejs-gantt [renderBaseline]="true" baselineColor="red"></ejs-gantt>
```

Baseline bars render as a separate indicator behind or below the current taskbar, visually showing schedule slippage.

---

## Work Scheduling (Effort-Driven)

Work-based scheduling calculates task duration based on assigned resource effort (hours of work):

```typescript
public taskFields: object = {
  id: 'TaskID',
  name: 'TaskName',
  startDate: 'StartDate',
  work: 'Work',               // Total work hours
  resourceInfo: 'resources'
};
```

```html
<ejs-gantt workUnit="Hour"></ejs-gantt>
```

Available `workUnit` values: `Day` | `Hour` | `Minute`

Configure the work week:
```typescript
public dayWorkingTime: object[] = [
  { from: 8, to: 12 },   // Morning shift
  { from: 13, to: 17 }   // Afternoon shift (excludes lunch)
];
```

```html
<ejs-gantt [dayWorkingTime]="dayWorkingTime"></ejs-gantt>
```

Work scheduling automatically recalculates duration when resource units change.
