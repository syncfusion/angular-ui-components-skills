# Selection

## Table of Contents
- [Overview](#overview)
- [Critical Rule](#critical-rule)
- [Row Selection](#row-selection)
- [Cell Selection](#cell-selection)
- [Column Selection](#column-selection)
- [Checkbox Selection](#checkbox-selection)
- [Selection Events](#selection-events)

## Overview

Grid selection allows users to select rows, cells, or columns. Configure selection mode and behavior to match your application's requirements.

## Critical Rule

#### Rule 1: Selection Methods Require [allowSelection]="true"

Selection methods like `selectRow()`, `selectRows()`, `selectAll()` **fail silently** without enabling selection first. Must set `[allowSelection]="true"` on the grid.

```typescript
// ❌ WRONG - selectRow() fails silently
import { Component, ViewChild } from '@angular/core';
import { GridComponent } from '@syncfusion/ej2-angular-grids';

@Component({
  selector: 'app-grid',
  template: `
    <ejs-grid #grid [dataSource]="data">
      <!-- allowSelection not set — defaults to false -->
      <e-columns>
        <e-column field="OrderID"></e-column>
      </e-columns>
    </ejs-grid>
  `
})
export class GridComponent {
  @ViewChild('grid') gridComponent!: GridComponent;
  
  selectFirstRow() {
    this.gridComponent.selectRow(0);  // ❌ FAILS SILENTLY - allowSelection is false
  }
}

// ✅ CORRECT - Enable selection first
@Component({
  selector: 'app-grid',
  template: `
    <ejs-grid #grid 
      [dataSource]="data" 
      [allowSelection]="true"
      [selectionSettings]="selectionSettings">
      <e-columns>
        <e-column field="OrderID"></e-column>
      </e-columns>
    </ejs-grid>
  `
})
export class GridComponent {
  @ViewChild('grid') gridComponent!: GridComponent;
  
  selectionSettings = {
    type: 'Multiple',
    mode: 'Row'
  };
  
  selectFirstRow() {
    this.gridComponent.selectRow(0);  // ✅ Works
  }
  
  selectMultipleRows() {
    this.gridComponent.selectRows([0, 2, 4]);  // ✅ Works
  }
  
  selectAll() {
    this.gridComponent.selectAll();  // ✅ Works
  }
}
```

**Always enable before using selection methods:**

```typescript
// Selection methods that require [allowSelection]="true":
this.gridComponent.selectRow(rowIndex)
this.gridComponent.selectRows([rowIndices])
this.gridComponent.selectAll()
this.gridComponent.clearSelection()
this.gridComponent.getSelectedRowIndexes()
this.gridComponent.getSelectedRecords()
```

---

## Row Selection

Enable and configure row selection:

```typescript
import { Component } from '@angular/core';
import { SelectionSettingsModel } from '@syncfusion/ej2-angular-grids';

@Component({
  selector: 'app-row-selection-grid',
  template: `
    <ejs-grid [dataSource]="data" 
              [selectionSettings]="selectionSettings"
              [allowSelection]="true">
      <e-columns>
        <e-column field="OrderID" headerText="Order ID" width="100"></e-column>
        <e-column field="CustomerName" headerText="Customer Name" width="150"></e-column>
        <e-column field="TotalAmount" headerText="Amount" type="number" format="C2" width="120"></e-column>
      </e-columns>
    </ejs-grid>
  `
})
export class RowSelectionGridComponent {
  selectionSettings: SelectionSettingsModel = {
    type: 'Multiple',  // 'Single', 'Multiple'
    mode: 'Row',       // 'Row', 'Cell', 'Both'
    checkboxOnly: false
  };

  data = [
    { OrderID: 10248, CustomerName: 'VINET', TotalAmount: 32.38 },
    { OrderID: 10249, CustomerName: 'TOMSP', TotalAmount: 11.61 },
    { OrderID: 10250, CustomerName: 'HANAR', TotalAmount: 65.83 }
  ];
}
```

## Cell Selection

Enable cell-level selection:

```typescript
import { Component } from '@angular/core';
import { SelectionSettingsModel } from '@syncfusion/ej2-angular-grids';

@Component({
  selector: 'app-cell-selection-grid',
  template: `
    <ejs-grid [dataSource]="data" [selectionSettings]="selectionSettings">
      <e-columns>
        <e-column field="OrderID" headerText="Order ID" width="100"></e-column>
        <e-column field="CustomerName" headerText="Customer Name" width="150"></e-column>
        <e-column field="Freight" headerText="Freight" type="number" format="C2" width="120"></e-column>
      </e-columns>
    </ejs-grid>
  `
})
export class CellSelectionGridComponent {
  selectionSettings: SelectionSettingsModel = {
    type: 'Multiple',  // Select multiple cells
    mode: 'Cell'       // Cell selection mode
  };

  data = [
    { OrderID: 10248, CustomerName: 'VINET', Freight: 32.38 },
    { OrderID: 10249, CustomerName: 'TOMSP', Freight: 11.61 }
  ];
}
```

## Column Selection

Select entire columns:

```typescript
import { Component } from '@angular/core';
import { SelectionSettingsModel } from '@syncfusion/ej2-angular-grids';

@Component({
  selector: 'app-column-selection-grid',
  template: `
    <ejs-grid [dataSource]="data" [selectionSettings]="selectionSettings">
      <e-columns>
        <e-column field="OrderID" headerText="Order ID" width="100"></e-column>
        <e-column field="CustomerName" headerText="Customer Name" width="150"></e-column>
        <e-column field="Freight" headerText="Freight" type="number" format="C2" width="120"></e-column>
      </e-columns>
    </ejs-grid>
  `
})
export class ColumnSelectionGridComponent {
  selectionSettings: SelectionSettingsModel = {
    type: 'Multiple',
    mode: 'Column'  // Column selection mode
  };

  data = [
    { OrderID: 10248, CustomerName: 'VINET', Freight: 32.38 },
    { OrderID: 10249, CustomerName: 'TOMSP', Freight: 11.61 }
  ];
}
```

## Checkbox Selection

Enable checkbox-based row selection:

```typescript
import { Component } from '@angular/core';
import { SelectionSettingsModel } from '@syncfusion/ej2-angular-grids';

@Component({
  selector: 'app-checkbox-selection-grid',
  template: `
    <ejs-grid [dataSource]="data" [selectionSettings]="selectionSettings">
      <e-columns>
        <e-column type="checkbox" width="50"></e-column>
        <e-column field="OrderID" headerText="Order ID" width="100"></e-column>
        <e-column field="CustomerName" headerText="Customer Name" width="150"></e-column>
        <e-column field="TotalAmount" headerText="Amount" type="number" format="C2" width="120"></e-column>
      </e-columns>
    </ejs-grid>
  `
})
export class CheckboxSelectionGridComponent {
  selectionSettings: SelectionSettingsModel = {
    type: 'Multiple',
    mode: 'Row',
    checkboxOnly: true  // Only checkboxes select rows
  };

  data = [
    { OrderID: 10248, CustomerName: 'VINET', TotalAmount: 32.38 },
    { OrderID: 10249, CustomerName: 'TOMSP', TotalAmount: 11.61 },
    { OrderID: 10250, CustomerName: 'HANAR', TotalAmount: 65.83 }
  ];
}
```

## Selection Events

Handle selection changes with events:

```typescript
import { Component } from '@angular/core';
import { RowSelectEventArgs, CellSelectEventArgs } from '@syncfusion/ej2-angular-grids';

@Component({
  selector: 'app-selection-events-grid',
  template: `
    <div>
      <p>Selected Rows: {{ selectedData | json }}</p>
    </div>
    <ejs-grid [dataSource]="data" 
              (rowSelected)="onRowSelected($event)"
              (cellSelected)="onCellSelected($event)">
      <e-columns>
        <e-column field="OrderID" headerText="Order ID" width="100"></e-column>
        <e-column field="CustomerName" headerText="Customer Name" width="150"></e-column>
      </e-columns>
    </ejs-grid>
  `
})
export class SelectionEventsGridComponent {
  selectedData: any[] = [];

  data = [
    { OrderID: 10248, CustomerName: 'VINET' },
    { OrderID: 10249, CustomerName: 'TOMSP' }
  ];

  onRowSelected(args: RowSelectEventArgs) {
    if (!this.selectedData.find(d => d.OrderID === args.data.OrderID)) {
      this.selectedData.push(args.data);
    }
    console.log('Row selected:', args.data);
  }

  onCellSelected(args: CellSelectEventArgs) {
    console.log('Cell selected:', args.value);
  }
}
```
