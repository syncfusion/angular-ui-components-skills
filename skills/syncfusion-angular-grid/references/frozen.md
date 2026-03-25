# Frozen

## Overview

Freeze columns to keep them visible while scrolling horizontally. Useful for ID or key columns that should always be visible.

## Freeze Default Columns

Freeze the first column:

```typescript
import { Component } from '@angular/core';
import { FrozenColumnsModel } from '@syncfusion/ej2-angular-grids';

@Component({
  selector: 'app-frozen-grid',
  template: `
    <ejs-grid [dataSource]="data" 
              [frozenColumns]="2"
              height="400">
      <e-columns>
        <e-column field="OrderID" headerText="Order ID" width="100"></e-column>
        <e-column field="CustomerName" headerText="Customer Name" width="150"></e-column>
        <e-column field="ShipCity" headerText="Ship City" width="150"></e-column>
        <e-column field="ShipAddress" headerText="Address" width="200"></e-column>
        <e-column field="Freight" headerText="Freight" type="number" format="C2" width="120"></e-column>
      </e-columns>
    </ejs-grid>
  `
})
export class FrozenGridComponent {
  data = [
    { OrderID: 10248, CustomerName: 'VINET', ShipCity: 'Reims', ShipAddress: '59 rue de l\'Abbaye', Freight: 32.38 },
    { OrderID: 10249, CustomerName: 'TOMSP', ShipCity: 'München', ShipAddress: 'Luisenstr. 48', Freight: 11.61 },
    { OrderID: 10250, CustomerName: 'HANAR', ShipCity: 'Rio de Janeiro', ShipAddress: 'Rua do Paço, 67', Freight: 65.83 }
  ];
}
```

## Freeze Specific Columns

Freeze specific columns instead of the first N:

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-frozen-specific-grid',
  template: `
    <ejs-grid [dataSource]="data" 
              [columns]="columns"
              height="400">
    </ejs-grid>
  `
})
export class FrozenSpecificGridComponent {
  columns = [
    { field: 'OrderID', headerText: 'Order ID', width: 100, isFrozen: true },
    { field: 'CustomerName', headerText: 'Customer Name', width: 150, isFrozen: true },
    { field: 'ShipCity', headerText: 'Ship City', width: 150 },
    { field: 'ShipAddress', headerText: 'Address', width: 200 },
    { field: 'Freight', headerText: 'Freight', type: 'number', format: 'C2', width: 120 }
  ];

  data = [
    { OrderID: 10248, CustomerName: 'VINET', ShipCity: 'Reims', ShipAddress: '59 rue de l\'Abbaye', Freight: 32.38 },
    { OrderID: 10249, CustomerName: 'TOMSP', ShipCity: 'München', ShipAddress: 'Luisenstr. 48', Freight: 11.61 },
    { OrderID: 10250, CustomerName: 'HANAR', ShipCity: 'Rio de Janeiro', ShipAddress: 'Rua do Paço, 67', Freight: 65.83 }
  ];
}
```
