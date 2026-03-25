# Timeline in Angular Gantt Chart

## Table of Contents
- [Timeline Settings Overview](#timeline-settings-overview)
- [Timeline View Modes](#timeline-view-modes)
- [Top and Bottom Tier Configuration](#top-and-bottom-tier-configuration)
- [Formatter Function Parameters](#formatter-function-parameters)
- [Timeline Cell Width](#timeline-cell-width)
- [Custom Date Formats](#custom-date-formats)
- [Project Date Range](#project-date-range)
- [Timeline View Dates](#timeline-view-dates)
- [Week Start Day](#week-start-day)
- [Automatic Timescale Update](#automatic-timescale-update)
- [Weekend Highlighting](#weekend-highlighting)
- [Timeline Cells Tooltip](#timeline-cells-tooltip)
- [Navigating the Timeline](#navigating-the-timeline)
- [Zooming](#zooming)
- [Timeline Template](#timeline-template)

---

## Timeline Settings Overview

The timeline is configured via `timelineSettings`, which controls the two-tier header (top and bottom) and cell width/count:

```typescript
public timelineSettings: object = {
  topTier: {
    unit: 'Week',
    format: 'MMM dd, yyyy',
    count: 1
  },
  bottomTier: {
    unit: 'Day',
    format: 'dd',
    count: 1
  },
  timelineUnitSize: 33   // Width in pixels of each bottom tier cell
};
```

---

## Timeline View Modes

Use `timelineViewMode` as a shortcut to configure both tiers at once:

| Mode | Top Tier | Bottom Tier |
|------|----------|-------------|
| `'Hour'` | Day | Hour |
| `'Day'` | Week | Day |
| `'Week'` | Month | Week |
| `'Month'` | Year | Month |
| `'Year'` | Year | Month |
| `'Minutes'` | Hour | Minute |

```html
<ejs-gantt timelineViewMode="Week"></ejs-gantt>
```

Or configure tiers explicitly for more control (see below).

---

## Top and Bottom Tier Configuration

Each tier supports `unit`, `format`, `count`, and `formatter`:

**Supported units:** `'Hour'` | `'Day'` | `'Week'` | `'Month'` | `'Year'` | `'Minutes'`

```typescript
public timelineSettings: object = {
  topTier: {
    unit: 'Month',
    format: 'MMMM yyyy',  // "April 2024"
    count: 1
  },
  bottomTier: {
    unit: 'Week',
    format: "'Week' W",   // "Week 14"
    count: 1
  }
};
```

The `count` property groups multiple units together. For example, `unit: 'Day', count: 7` shows weeks as a single cell in the bottom tier.

---

## Custom Date Formats

Use standard date format strings:

| Token | Meaning | Example |
|-------|---------|---------|
| `MMM` | Month abbreviation | Apr |
| `MMMM` | Full month name | April |
| `MM` | Month number (padded) | 04 |
| `dd` | Day (padded) | 02 |
| `yyyy` | 4-digit year | 2024 |
| `ddd` | Day name abbreviation | Mon |
| `W` | Week number | 14 |

---

## Formatter Function Parameters

The `formatter` function receives four arguments: `date`, `format`, `tier`, and `mode`:

```typescript
public timelineSettings: object = {
  bottomTier: {
    unit: 'Day',
    formatter: (date: Date, format: string, tier: string, mode: string) => {
      // tier: 'topTier' | 'bottomTier'
      // mode: unit string like 'Day', 'Week', 'Month', etc.
      return date.toLocaleDateString('en-US', { weekday: 'short', day: 'numeric' });
    }
  }
};
```

Use `tier` to differentiate formatting logic between top and bottom tiers, and `mode` to know which timeline unit is active.

---

## Timeline Cell Width

Control the width of each bottom-tier cell in pixels using `timelineUnitSize`:

```typescript
public timelineSettings: object = {
  bottomTier: { unit: 'Day', format: 'dd' },
  timelineUnitSize: 50   // Default is 33px; increase for more spacing
};
```

Top-tier cells automatically span the combined width of the bottom-tier cells they cover.

---

## Project Date Range

Define the timeline start and end to avoid auto-calculation from task data:

```html
<ejs-gantt
  projectStartDate="03/01/2024"
  projectEndDate="09/30/2024">
</ejs-gantt>
```

Without these properties, the Gantt derives the date range from the earliest and latest task dates. Setting them explicitly ensures consistent timeline rendering even when tasks haven't been assigned yet.

---

## Timeline View Dates

Use `viewStartDate` and `viewEndDate` inside `timelineSettings` to show a fixed visible window of the timeline without altering the project date range:

```typescript
public timelineSettings: object = {
  viewStartDate: new Date('03/01/2024'),
  viewEndDate: new Date('06/30/2024')
};
```

- The timeline header renders only within this window even if tasks extend beyond it.
- `projectStartDate` / `projectEndDate` still control the full project scope; `viewStartDate` / `viewEndDate` only affect what is visible.

---

## Week Start Day

Configure which day is treated as the start of the week using `weekStartDay` (0 = Sunday by default):

```typescript
public timelineSettings: object = {
  topTier: { unit: 'Week', format: 'MMM dd, yyyy' },
  bottomTier: { unit: 'Day', format: 'dd' },
  weekStartDay: 1   // 0=Sunday, 1=Monday, ... 6=Saturday
};
```

---

## Automatic Timescale Update

When tasks are edited and their dates move beyond the current timeline range, the Gantt can auto-extend the timeline. This is controlled by `updateTimescaleView`:

```typescript
public timelineSettings: object = {
  updateTimescaleView: true   // Default: true — auto-extends timeline on task edit
};
```

Set to `false` to lock the timeline range and prevent it from shifting when tasks are dragged or resized beyond the visible area.

---

## Zooming

Enable zoom toolbar buttons to let users adjust timeline granularity:

```typescript
public toolbar: string[] = ['ZoomIn', 'ZoomOut', 'ZoomToFit'];
```

Programmatic zoom:
```typescript
this.ganttObj.zoomIn();    // Increase detail level
this.ganttObj.zoomOut();   // Decrease detail level
this.ganttObj.fitToProject(); // Fit entire project in view
```

Configure zoom levels:
```typescript
public zoomingLevels: object[] = [
  { topTier: { unit: 'Year', format: 'yyyy', count: 1 }, bottomTier: { unit: 'Month', format: 'MMM', count: 1 }, timelineUnitSize: 33, level: 1 },
  { topTier: { unit: 'Month', format: 'MMM yyyy', count: 1 }, bottomTier: { unit: 'Week', format: "'W'W", count: 1 }, timelineUnitSize: 33, level: 2 },
  { topTier: { unit: 'Week', format: 'MMM dd, yyyy', count: 1 }, bottomTier: { unit: 'Day', format: 'dd', count: 1 }, timelineUnitSize: 33, level: 3 }
];
```

```html
<ejs-gantt [zoomingLevels]="zoomingLevels"></ejs-gantt>
```

---

## Weekend Highlighting

Highlight non-working day columns in the timeline using `showWeekend` inside `timelineSettings`. Use `workWeek` to define which days are working:

```typescript
public timelineSettings: object = {
  showWeekend: true   // Highlights non-working days (based on workWeek)
};

public workWeek: string[] = ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday'];
```

```html
<ejs-gantt [timelineSettings]="timelineSettings" [workWeek]="workWeek"></ejs-gantt>
```

---

## Timeline Cells Tooltip

Hovering over timeline cells shows a tooltip with date information by default. Disable it via `showTooltip`:

```typescript
public timelineSettings: object = {
  showTooltip: false   // Default: true
};
```

---

## Navigating the Timeline

Move the visible timeline window forward or backward by one time unit programmatically:

```typescript
// Move timeline forward by one unit
this.ganttObj.nextTimeSpan();

// Move timeline backward by one unit
this.ganttObj.previousTimeSpan();
```

These methods extend the `projectStartDate` / `projectEndDate` of the Gantt, effectively scrolling the timeline. Useful for toolbar buttons or navigation controls.

---

## Timeline Template

Customize the rendering of tier cells using templates:

```typescript
public timelineSettings: object = {
  topTier: {
    unit: 'Week',
    formatter: (date: Date) => {
      const week = getWeekNumber(date);
      return `<b>Week ${week}</b>`;
    }
  }
};
```

The formatter receives `(date: Date, format: string, tier: string, mode: string)` and returns an HTML string for the cell content.
