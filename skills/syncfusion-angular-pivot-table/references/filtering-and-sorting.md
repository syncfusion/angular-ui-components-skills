# Filtering & Sorting

## Table of Contents
- [Member Filtering](#member-filtering)
- [Label Filtering](#label-filtering)
- [Value Filtering](#value-filtering)
- [Member Sorting](#member-sorting)
- [Value Sorting](#value-sorting)
- [Sorting Events](#sorting-events)

---

## Member Filtering

Include or exclude specific field members from the pivot table.

### Enable Member Filtering

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-filter',
  template: `<ejs-pivotview [dataSourceSettings]="dataSourceSettings"></ejs-pivotview>`
})
export class AppComponent {
  dataSourceSettings: IDataOptions = {
    dataSource: data,
    rows: [{ name: 'Country' }],
    columns: [{ name: 'Year' }],
    values: [{ name: 'Sales', type: 'Sum' }],
    filterSettings: [{
      name: 'Country',
      type: 'Include',
      items: ['USA', 'Canada']
    }]
  };
}
```

## Label Filtering

Filter based on header text or member names.

```typescript
filterSettings: [{
  name: 'Country',
  type: 'Label',
  condition: 'Contains',
  value1: 'United'
}]
```

## Value Filtering

Filter based on aggregated values meeting specific conditions.

```typescript
filterSettings: [{
  name: 'Country',
  type: 'Value',
  measure: 'Sales',
  condition: 'GreaterThan',
  value1: '1500'
}]
```

---

## Member Sorting

Arrange field members in ascending or descending order. Enabled by default.

### Basic Member Sorting

```typescript
sortSettings: [{
  name: 'Year',
  order: 'Ascending'  // or 'Descending'
}]
```

### Alphanumeric Sorting

Sort members numerically by their numeric prefix instead of alphabetically:

```typescript
rows: [{
  name: 'ProductCode',
  dataType: 'number'  // Sorts '36-SW', '71-AJ', '209-FB' instead of '209-FB', '36-SW', '71-AJ'
}]
```

### Custom Member Sorting

Sort field members in user-defined order:

```typescript
sortSettings: [{
  name: 'Country',
  membersOrder: ['USA', 'Canada', 'France', 'Germany'],
  order: 'Ascending'
}]
```

**Key Properties:**
- `name`: Field to sort
- `membersOrder`: Array of members in desired order
- `order`: Direction (Ascending/Descending/None)

---

## Value Sorting

Sort pivot table values and aggregated data in ascending or descending order.

### Enable Value Sorting

```typescript
@Component({
  template: `<ejs-pivotview 
    [dataSourceSettings]="dataSourceSettings"
    [enableValueSorting]="true">
  </ejs-pivotview>`
})
export class AppComponent {
  dataSourceSettings: IDataOptions = {
    dataSource: data,
    rows: [{ name: 'Country' }],
    columns: [{ name: 'Year' }],
    values: [{ name: 'Sales', type: 'Sum' }]
  };
}
```

### Programmatic Value Sorting

```typescript
valueSortSettings = {
  headerText: 'FY 2020##Q1',    // Column hierarchy path
  headerDelimiter: '##',         // Delimiter between levels
  sortOrder: 'Descending'        // Sort direction
};
```

### Multiple Axis Value Sorting

Sort values simultaneously in row and column axes:

```typescript
valueSortSettings = {
  columnHeaderText: 'FY 2020##Q1',
  rowHeaderText: 'USA',
  headerDelimiter: '##',
  columnSortOrder: 'Descending',
  rowSortOrder: 'Ascending'
};
```

---

## Sorting Events

### onHeadersSort Event

Triggered when headers are sorted. Allows customization of member order:

```typescript
onHeadersSort(args: HeadersSortEventArgs) {
  if (args.fieldName === 'Country') {
    args.members = args.members.reverse();
    args.isOrderChanged = true;
  }
}
```

### actionBegin Event

Triggered when sort action starts:

```typescript
actionBegin(args: PivotActionBeginEventArgs) {
  if (args.actionName === 'Sort field' || args.actionName === 'Sort value') {
    // Prevent sort action
    args.cancel = true;
  }
}
```

### actionComplete Event

Triggered when sort action completes:

```typescript
actionComplete(args: PivotActionCompleteEventArgs) {
  if (args.actionName === 'Sort field') {
    console.log('Sorting completed', args.dataSourceSettings);
  }
}
```

---

## Best Practices

1. **Enable Value Sorting** for financial/analytical reports
2. **Use Custom Sorting** for business category orders
3. **Combine with Filtering** for focused analysis
4. **Use Events** to prevent unwanted sort operations
5. **Limit Sort Levels** for performance with large datasets

### Filtering Events

```typescript
(memberFiltering)="onMemberFiltering($event)"
onMemberFiltering(args) {
  args.cancel = true; // Cancel filter
}
```

### Sorting Events

```typescript
(onHeadersSort)="onHeadersSort($event)"
onHeadersSort(args) {
  args.members = ['Custom', 'Order'];
  args.isOrderChanged = true;
}
```