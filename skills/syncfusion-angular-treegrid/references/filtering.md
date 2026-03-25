---
name: Filtering
description: 'Filtering in Syncfusion Angular TreeGrid - filter bar mode, filter menu mode, excel-like filtering, custom filters, and filter predicates.'
---

# Filtering

Filtering enables users to find specific records based on column values using multiple filter modes.

## Table of Contents
- [Basic Filtering](#basic-filtering)
- [Filter Modes](#filter-modes)
- [Custom Filters](#custom-filters)
- [Filter Events](#filter-events)
- [Server-side Filtering](#server-side-filtering)

## Basic Filtering

### Enable Filtering

```typescript
import { Component } from '@angular/core';
import { FilterService } from '@syncfusion/ej2-angular-treegrid';

@Component({
  selector: 'app-treegrid',
  template: `
    <ejs-treegrid 
      [dataSource]='data'
      [childMapping]='childMapping'
      [allowFiltering]='true'
      [filterSettings]='filterSettings'>
      <e-columns>
        <e-column field='TaskID' headerText='Task ID' width='90' type='number'></e-column>
        <e-column field='TaskName' headerText='Task Name' width='200'></e-column>
        <e-column field='Status' headerText='Status' width='100'></e-column>
      </e-columns>
    </ejs-treegrid>
  `,
  providers: [FilterService]
})
export class AppComponent {
  public data: Object[] = [];
  public childMapping: string = 'subtasks';
  
  public filterSettings = {
    type: 'FilterBar'
  };
}
```

## Filter Modes

### Filter Bar

Users type in the filter row:

```typescript
public filterSettings = {
  type: 'FilterBar'
};
```

### Filter Menu

Dropdown with filter options:

```typescript
public filterSettings = {
  type: 'Menu'
};
```

### Excel-like Filter

Excel-style filtering interface:

```typescript
public filterSettings = {
  type: 'Excel'
};
```

## Custom Filters

### Filter by Column

```typescript
@ViewChild('treegrid') treegrid!: TreeGridComponent;

filterByColumn(field: string, operator: string, value: any) {
  this.treegrid.filterByColumn(field, operator, value);
}

// Usage
filterByColumn('Status', 'equal', 'Completed');
```

### Programmatic Filtering

```typescript
<button (click)='applyFilter()'>Filter Data</button>

<ejs-treegrid 
  #treegrid
  [dataSource]='data'
  [childMapping]='childMapping'
  [allowFiltering]='true'>
  <!-- columns -->
</ejs-treegrid>
```

```typescript
applyFilter() {
  this.treegrid.filterByColumn('Progress', 'greaterThan', 50);
}
```

### Clear Filters

```typescript
clearFilters() {
  this.treegrid.clearFiltering();
}
```

### Custom Filter Template

```typescript
<ejs-treegrid 
  [dataSource]='data'
  [childMapping]='childMapping'
  [allowFiltering]='true'>
  <e-columns>
    <e-column field='Status' 
      headerText='Status' 
      [filter]='statusFilterTemplate'
      width='120'>
    </e-column>
  </e-columns>
</ejs-treegrid>

<ng-template #statusFilterTemplate let-data>
  <select style="width: 100%;">
    <option value="">All</option>
    <option value="Completed">Completed</option>
    <option value="InProgress">In Progress</option>
    <option value="Pending">Pending</option>
  </select>
</ng-template>
```

### Predicate-based Filtering

```typescript
import { Predicate } from '@syncfusion/ej2-data';

filterWithPredicates() {
  const predicate1 = new Predicate('Status', 'equal', 'Completed');
  const predicate2 = new Predicate('Priority', 'equal', 'High');
  const finalPredicate = predicate1.or(predicate2);
  
  this.treegrid.filterByColumn(finalPredicate);
}
```

### Date Range Filter

```typescript
<ejs-treegrid 
  [dataSource]='data'
  [childMapping]='childMapping'
  [allowFiltering]='true'>
  <!-- columns -->
</ejs-treegrid>

<div style="margin-bottom: 20px;">
  <label>From: <input type='date' [(ngModel)]='startDate'></label>
  <label>To: <input type='date' [(ngModel)]='endDate'></label>
  <button (click)='filterByDateRange()'>Filter</button>
</div>
```

```typescript
startDate: string = '';
endDate: string = '';

filterByDateRange() {
  const predicate1 = new Predicate('TaskDate', 'greaterThanOrEqual', this.startDate);
  const predicate2 = new Predicate('TaskDate', 'lessThanOrEqual', this.endDate);
  const finalPredicate = predicate1.and(predicate2);
  
  this.treegrid.filterByColumn(finalPredicate);
}
```

## Filter Events

### Before Filter

```typescript
<ejs-treegrid 
  (actionBegin)='onActionBegin($event)'>
  <!-- columns -->
</ejs-treegrid>
```

```typescript
onActionBegin(args: any) {
  if (args.requestType === 'filtering') {
    console.log('Filtering started:', args.filterModel);
  }
}
```

### After Filter

```typescript
<ejs-treegrid 
  (actionComplete)='onActionComplete($event)'>
  <!-- columns -->
</ejs-treegrid>
```

```typescript
onActionComplete(args: any) {
  if (args.requestType === 'filtering') {
    console.log('Filtered records:', this.treegrid.records.length);
  }
}
```

### Prevent Filter

```typescript
onActionBegin(args: any) {
  if (args.requestType === 'filtering') {
    if (args.column?.field === 'SecureField') {
      args.cancel = true;  // Prevent filtering on secure field
    }
  }
}
```

## Server-side Filtering

### Filter on Server

```typescript
import { Component, ViewChild } from '@angular/core';
import { TreeGridComponent, FilterService } from '@syncfusion/ej2-angular-treegrid';
import { DataManager, UrlAdaptor } from '@syncfusion/ej2-data';

@Component({
  selector: 'app-treegrid',
  template: `
    <ejs-treegrid 
      #treegrid
      [dataSource]='dataManager'
      idMapping='TaskID'
      parentIdMapping='ParentItem'
      hasChildMapping='isParent'
      [allowFiltering]='true'
      [filterSettings]='filterSettings'
      (actionComplete)='onActionComplete($event)'>
      <e-columns>
        <e-column field='TaskID' headerText='Task ID' width='90'></e-column>
        <e-column field='TaskName' headerText='Task Name' width='200'></e-column>
        <e-column field='Status' headerText='Status' width='100'></e-column>
        <e-column field='Priority' headerText='Priority' width='100'></e-column>
      </e-columns>
    </ejs-treegrid>
  `,
  providers: [FilterService]
})
export class AppComponent {
  @ViewChild('treegrid') treegrid!: TreeGridComponent;
  
  public dataManager: DataManager;
  public filterSettings = {
    type: 'FilterBar',
    mode: 'Immediate'  // Immediate on server
  };

  constructor() {
    this.dataManager = new DataManager({
      url: 'https://api.example.com/tasks',
      adaptor: new UrlAdaptor(),
      // Server handles filtering via query parameters
      params: []
    });
  }

  onActionComplete(args: any): void {
    if (args.requestType === 'filtering') {
      console.log('Server-side filter applied');
    }
  }
}
```
