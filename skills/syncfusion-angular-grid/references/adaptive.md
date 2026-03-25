# Adaptive

## Overview

Adaptive mode provides responsive grid layout for mobile and tablet devices. Grid automatically adjusts column visibility and behavior based on screen size.

## Responsive Grid Layout

Enable adaptive/responsive mode:

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-adaptive-grid',
  template: `
    <p style="padding: 10px; color: #666;">
      Resize browser or view on mobile to see adaptive behavior
    </p>
    <div class="e-bigger">
      <ejs-grid [dataSource]="data" 
                [enableAdaptiveUI]="true"
                [allowPaging]="true"
                height="400">
        <e-columns>
          <e-column field="OrderID" headerText="Order ID" width="100" minWidth="50"></e-column>
          <e-column field="CustomerName" headerText="Customer Name" width="150" minWidth="80"></e-column>
          <e-column field="ShipCity" headerText="Ship City" width="150" minWidth="80"></e-column>
          <e-column field="Freight" headerText="Freight" type="number" format="C2" width="120" minWidth="60"></e-column>
        </e-columns>
      </ejs-grid>
    </div>
  `
})
export class AdaptiveGridComponent {
  data = [
    { OrderID: 10248, CustomerName: 'VINET', ShipCity: 'Reims', Freight: 32.38 },
    { OrderID: 10249, CustomerName: 'TOMSP', ShipCity: 'München', Freight: 11.61 },
    { OrderID: 10250, CustomerName: 'HANAR', ShipCity: 'Rio de Janeiro', Freight: 65.83 },
    { OrderID: 10251, CustomerName: 'VICTE', ShipCity: 'Rio de Janeiro', Freight: 41.34 }
  ];
}
```
