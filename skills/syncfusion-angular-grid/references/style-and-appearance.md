# Style and Appearance

## Table of Contents
- [Overview](#overview)
- [Theme Selection](#theme-selection)
- [CSS Customization](#css-customization)
- [Style Specific Areas](#style-specific-areas)
- [Custom CSS Classes](#custom-css-classes)

## Overview

Customize grid appearance through themes, CSS, and custom styling. Create professional-looking data tables that match your brand.

## Theme Selection

Choose from built-in themes:

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-theme-grid',
  template: `
    <div style="margin-bottom: 15px;">
      <button *ngFor="let theme of themes" (click)="changeTheme(theme)"
              [style.font-weight]="currentTheme === theme ? 'bold' : 'normal'">
        {{ theme }}
      </button>
    </div>
    
    <ejs-grid [dataSource]="data" 
              [allowPaging]="true">
      <e-columns>
        <e-column field="OrderID" headerText="Order ID" width="100"></e-column>
        <e-column field="CustomerName" headerText="Customer Name" width="150"></e-column>
        <e-column field="Freight" headerText="Freight" type="number" format="C2" width="120"></e-column>
      </e-columns>
    </ejs-grid>
  `
})
export class ThemeGridComponent {
  currentTheme = 'material';
  themes = ['material', 'fabric', 'bootstrap', 'bootstrap5', 'tailwind', 'fluent'];

  data = [
    { OrderID: 10248, CustomerName: 'VINET', Freight: 32.38 },
    { OrderID: 10249, CustomerName: 'TOMSP', Freight: 11.61 },
    { OrderID: 10250, CustomerName: 'HANAR', Freight: 65.83 }
  ];

  changeTheme(theme: string) {
    this.currentTheme = theme;
    // Dynamically load theme CSS
    const link = document.createElement('link');
    link.rel = 'stylesheet';
    link.href = `@syncfusion/ej2-grids/styles/${theme}.css`;
    document.head.appendChild(link);
  }
}
```

## CSS Customization

Customize grid with CSS:

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-custom-css-grid',
  template: `
    <style>
      :host ::ng-deep .e-grid {
        font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      }

      :host ::ng-deep .e-gridheader {
        background-color: #2196F3;
        color: white;
      }

      :host ::ng-deep .e-headercell {
        font-weight: 600;
        padding: 12px;
      }

      :host ::ng-deep .e-rowcell {
        padding: 10px;
        border-right: 1px solid #f0f0f0;
      }

      :host ::ng-deep .e-row:hover {
        background-color: #f5f5f5;
        cursor: pointer;
      }

      :host ::ng-deep .e-row.e-selectionbackground {
        background-color: #bbdefb;
      }

      :host ::ng-deep .e-pagercontainer {
        background-color: #fafafa;
        border-top: 1px solid #e0e0e0;
      }
    </style>
    
    <ejs-grid [dataSource]="data" [allowPaging]="true">
      <e-columns>
        <e-column field="OrderID" headerText="Order ID" width="100"></e-column>
        <e-column field="CustomerName" headerText="Customer Name" width="150"></e-column>
        <e-column field="Freight" headerText="Freight" type="number" format="C2" width="120"></e-column>
      </e-columns>
    </ejs-grid>
  `
})
export class CustomCssGridComponent {
  data = [
    { OrderID: 10248, CustomerName: 'VINET', Freight: 32.38 },
    { OrderID: 10249, CustomerName: 'TOMSP', Freight: 11.61 },
    { OrderID: 10250, CustomerName: 'HANAR', Freight: 65.83 }
  ];
}
```

## Style Specific Areas

Style headers, rows, and cells individually:

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-area-style-grid',
  template: `
    <style>
      /* Header styling */
      :host ::ng-deep .e-gridheader .e-headercell {
        background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
        color: white;
        font-weight: 600;
      }

      /* Alternating row colors -->
      :host ::ng-deep .e-row:nth-child(odd) {
        background-color: #f9f9f9;
      }

      /* Highlight important column */
      :host ::ng-deep .e-grid td[e-mappinguid='Freight'] {
        background-color: #fffacd;
        font-weight: 500;
      }

      /* Footer/pager styling -->
      :host ::ng-deep .e-pagercontainer {
        background: linear-gradient(to right, #f5f7fa 0%, #c3cfe2 100%);
      }
    </style>
    
    <ejs-grid [dataSource]="data" 
              [allowPaging]="true"
              [allowSorting]="true">
      <e-columns>
        <e-column field="OrderID" headerText="Order ID" width="100"></e-column>
        <e-column field="CustomerName" headerText="Customer Name" width="150"></e-column>
        <e-column field="Freight" headerText="Freight" type="number" format="C2" width="120"></e-column>
      </e-columns>
    </ejs-grid>
  `
})
export class AreaStyleGridComponent {
  data = [
    { OrderID: 10248, CustomerName: 'VINET', Freight: 32.38 },
    { OrderID: 10249, CustomerName: 'TOMSP', Freight: 11.61 },
    { OrderID: 10250, CustomerName: 'HANAR', Freight: 65.83 },
    { OrderID: 10251, CustomerName: 'VICTE', Freight: 41.34 }
  ];
}
```

## Custom CSS Classes

Apply custom classes to rows and cells:

```typescript
import { Component } from '@angular/core';
import { QueryCellInfoEventArgs } from '@syncfusion/ej2-angular-grids';

@Component({
  selector: 'app-custom-class-grid',
  template: `
    <style>
      :host ::ng-deep .high-freight {
        background-color: #c8e6c9 !important;
        font-weight: 500;
      }

      :host ::ng-deep .low-freight {
        background-color: #ffcccc !important;
      }

      :host ::ng-deep .warning-row {
        background-color: #fff9c4;
      }
    </style>
    
    <ejs-grid [dataSource]="data" 
              (queryCellInfo)="onQueryCellInfo($event)">
      <e-columns>
        <e-column field="OrderID" headerText="Order ID" width="100"></e-column>
        <e-column field="CustomerName" headerText="Customer Name" width="150"></e-column>
        <e-column field="Freight" headerText="Freight" type="number" format="C2" width="120"></e-column>
      </e-columns>
    </ejs-grid>
  `
})
export class CustomClassGridComponent {
  data = [
    { OrderID: 10248, CustomerName: 'VINET', Freight: 32.38 },
    { OrderID: 10249, CustomerName: 'TOMSP', Freight: 11.61 },
    { OrderID: 10250, CustomerName: 'HANAR', Freight: 65.83 },
    { OrderID: 10251, CustomerName: 'VICTE', Freight: 41.34 }
  ];

  onQueryCellInfo(args: QueryCellInfoEventArgs) {
    const data = args.data as any;
    if (data.Freight > 50) {
      args.cell?.classList.add('high-freight');
    } else if (data.Freight < 20) {
      args.cell?.classList.add('low-freight');
    }
  }
}
```
