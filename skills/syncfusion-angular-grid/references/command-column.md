# Command Column in Angular Grid

## Table of Contents
- [Overview](#overview)
- [Enable Command Column](#enable-command-column)
- [Built-in Commands](#built-in-commands)
- [Custom Commands](#custom-commands)
- [Command Events](#command-events)
- [Conditional Commands](#conditional-commands)
- [Styling Commands](#styling-commands)

## Overview

Command column provides buttons for common grid actions like Edit, Delete, and custom operations. It simplifies row-level interactions without needing custom templates.

---

## Enable Command Column

### Basic Setup

```typescript
import { Component } from '@angular/core';
import { EditService, ToolbarService } from '@syncfusion/ej2-angular-grids';

@Component({
  selector: 'app-grid',
  template: `
    <ejs-grid [dataSource]="data"
              [editSettings]="{ mode: 'Dialog', allowEditing: true, allowDeleting: true }">
      <e-columns>
        <e-column type="checkbox" width="50"></e-column>
        <e-column field="OrderID" headerText="Order ID" width="100" [isPrimaryKey]="true"></e-column>
        <e-column field="CustomerID" headerText="Customer" width="120"></e-column>
        <e-column type="commandColumn" headerText="Actions" width="150"
                  [commands]="[{type: 'Edit', buttonOption: {cssClass: 'e-flat'}},
                               {type: 'Delete', buttonOption: {cssClass: 'e-flat'}},
                               {type: 'Save', buttonOption: {cssClass: 'e-flat'}},
                               {type: 'Cancel', buttonOption: {cssClass: 'e-flat'}}]">
        </e-column>
      </e-columns>
    </ejs-grid>
  `,
  providers: [EditService, ToolbarService]
})
export class AppCommandGridComponent {
  data = [];
}
```

---

## Built-in Commands

### Available Built-in Commands

| Command | Available In | Action |
|---------|--------------|--------|
| `Edit` | Normal mode | Start editing row |
| `Delete` | Normal mode | Delete row |
| `Save` | Edit mode | Save changes |
| `Cancel` | Edit mode | Cancel editing |

### Edit/Delete Commands

```typescript
<e-column
  headerText="Edit/Delete"
  width="120"
  [commands]="[
    { type: 'Edit', buttonOption: { cssClass: 'e-flat' } },
    { type: 'Delete', buttonOption: { cssClass: 'e-flat' } }
  ]">
</e-column>
```

### Save/Cancel Commands

```typescript
<e-column
  headerText="Save/Cancel"
  width="120"
  [commands]="[
    { type: 'Save', buttonOption: { cssClass: 'e-flat' } },
    { type: 'Cancel', buttonOption: { cssClass: 'e-flat' } }
  ]">
</e-column>
```

### All Commands

```typescript
<e-column
  headerText="Actions"
  width="200"
  [commands]="[
    { type: 'Edit', buttonOption: { cssClass: 'e-flat', iconCss: 'e-icons e-edit' } },
    { type: 'Save', buttonOption: { cssClass: 'e-flat', iconCss: 'e-icons e-save' } },
    { type: 'Cancel', buttonOption: { cssClass: 'e-flat', iconCss: 'e-icons e-cancel' } },
    { type: 'Delete', buttonOption: { cssClass: 'e-flat e-flat-danger', iconCss: 'e-icons e-delete' } }
  ]">
</e-column>
```

---

## Custom Commands

### Duplicate Row Command

```typescript
import { Component, ViewChild } from '@angular/core';
import { GridComponent, EditService } from '@syncfusion/ej2-angular-grids';

@Component({
  selector: 'app-duplicate-command',
  template: `
    <ejs-grid #grid [dataSource]="data" [editSettings]="editSettings">
      <e-columns>
        <e-column field="OrderID" headerText="Order ID" [isPrimaryKey]="true"></e-column>
        <e-column field="CustomerID" headerText="Customer"></e-column>
        <e-column
          headerText="Actions"
          width="150"
          [commands]="[
            { type: 'Edit', buttonOption: { cssClass: 'e-flat' } },
            { buttonOption: { content: 'Copy', cssClass: 'e-flat', click: onDuplicate } },
            { type: 'Delete', buttonOption: { cssClass: 'e-flat' } }
          ]">
        </e-column>
      </e-columns>
    </ejs-grid>
  `,
  providers: [EditService]
})
export class DuplicateCommandComponent {
  @ViewChild('grid') gridComponent!: GridComponent;
  data: any[] = [];
  editSettings: any = { allowEditing: true, allowDeleting: true };

  onDuplicate = (args: any): void => {
    const rowData = args.rowData;
    const newData = { ...rowData };
    delete newData[this.gridComponent.getPrimaryKeyFieldNames()[0]];
    
    this.gridComponent.addRecord(newData);
  };
```

### View Details Command

```typescript
import { Component, ViewChild } from '@angular/core';
import { GridComponent, EditService } from '@syncfusion/ej2-angular-grids';

@Component({
  selector: 'app-details-command',
  template: `
    <ejs-grid #grid [dataSource]="data" [editSettings]="editSettings">
      <e-columns>
        <e-column field="OrderID" headerText="Order ID" [isPrimaryKey]="true"></e-column>
        <e-column field="CustomerID" headerText="Customer"></e-column>
        <e-column
          headerText="Actions"
          width="150"
          [commands]="[
            { buttonOption: { content: 'Details', cssClass: 'e-flat e-info', click: onViewDetails } },
            { type: 'Edit', buttonOption: { cssClass: 'e-flat' } },
            { type: 'Delete', buttonOption: { cssClass: 'e-flat' } }
          ]">
        </e-column>
      </e-columns>
    </ejs-grid>
  `,
  providers: [EditService]
})
export class DetailsCommandComponent {
  @ViewChild('grid') gridComponent!: GridComponent;
  data: any[] = [];
  editSettings: any = { allowEditing: true, allowDeleting: true };

  onViewDetails = (args: any): void => {
    const rowData = args.rowData;
    this.showDetailModal(rowData);
  };

  private showDetailModal(data: any): void {
    alert(`Order Details: ${JSON.stringify(data)}`);
  }
```

### Download/Export Command

```typescript
import { Component, ViewChild } from '@angular/core';
import { GridComponent, EditService } from '@syncfusion/ej2-angular-grids';

@Component({
  selector: 'app-download-command',
  template: `
    <ejs-grid #grid [dataSource]="data" [editSettings]="editSettings">
      <e-columns>
        <e-column field="OrderID" headerText="Order ID" [isPrimaryKey]="true"></e-column>
        <e-column field="CustomerID" headerText="Customer"></e-column>
        <e-column
          headerText="Actions"
          width="150"
          [commands]="[
            { buttonOption: { content: 'Download', cssClass: 'e-flat e-success', iconCss: 'e-icons e-download', click: onDownload } },
            { type: 'Edit', buttonOption: { cssClass: 'e-flat' } }
          ]">
        </e-column>
      </e-columns>
    </ejs-grid>
  `,
  providers: [EditService]
})
export class DownloadCommandComponent {
  @ViewChild('grid') gridComponent!: GridComponent;
  data: any[] = [];
  editSettings: any = { allowEditing: true, allowDeleting: true };

  onDownload = (args: any): void => {
    const rowData = args.rowData;
    const csv = this.convertToCSV(rowData);
    this.downloadCSV(csv, `order-${rowData.OrderID}.csv`);
  };

  private convertToCSV(data: any): string {
    return JSON.stringify(data);
  }

  private downloadCSV(csv: string, filename: string): void {
    const element = document.createElement('a');
    element.setAttribute('href', 'data:text/csv;charset=utf-8,' + encodeURIComponent(csv));
    element.setAttribute('download', filename);
    element.style.display = 'none';
    document.body.appendChild(element);
    element.click();
    document.body.removeChild(element);
  }
```

### Multi-Action Button

```typescript
import { Component, ViewChild } from '@angular/core';
import { GridComponent, EditService } from '@syncfusion/ej2-angular-grids';

@Component({
  selector: 'app-multi-action-command',
  template: `
    <ejs-grid #grid [dataSource]="data" [editSettings]="editSettings">
      <e-columns>
        <e-column field="OrderID" headerText="Order ID" [isPrimaryKey]="true"></e-column>
        <e-column field="CustomerID" headerText="Customer"></e-column>
        <e-column
          headerText="Actions"
          width="150"
          [commands]="commandOptions">
        </e-column>
      </e-columns>
    </ejs-grid>
  `,
  providers: [EditService]
})
export class MultiActionCommandComponent {
  @ViewChild('grid') gridComponent!: GridComponent;
  data: any[] = [];
  editSettings: any = { allowEditing: true, allowDeleting: true };

  commandOptions = [
    {
      buttonOption: {
        content: 'More Actions',
        cssClass: 'e-flat e-outline',
        click: this.showActionMenu
      }
    }
  ];

  private showActionMenu = (args: any): void => {
    const actions = [
      { label: 'Print', action: () => this.printRow(args.rowData) },
      { label: 'Email', action: () => this.emailRow(args.rowData) },
      { label: 'Archive', action: () => this.archiveRow(args.rowData) },
      { label: 'Share', action: () => this.shareRow(args.rowData) }
    ];
    
    actions.forEach(action => {
      const button = document.createElement('button');
      button.textContent = action.label;
      button.className = 'e-btn e-flat';
      button.onclick = () => action.action();
      document.body.appendChild(button);
    });
  };

  private printRow(data: any): void {
    console.log('Print:', data);
  }

  private emailRow(data: any): void {
    console.log('Email:', data);
  }

  private archiveRow(data: any): void {
    console.log('Archive:', data);
  }

  private shareRow(data: any): void {
    console.log('Share:', data);
  }
```

---

## Command Events

### Click Event

```typescript
import { Component, ViewChild, OnInit } from '@angular/core';
import { GridComponent } from '@syncfusion/ej2-angular-grids';

@Component({
  selector: 'app-command-click-grid',
  template: `
    <ejs-grid #grid [dataSource]="data" (commandClick)="onCommandClick($event)">
      <e-columns>
        <!-- columns with commands -->
      </e-columns>
    </ejs-grid>
  `
})
export class CommandClickGridComponent implements OnInit {
  @ViewChild('grid') gridInstance: GridComponent;
  data: any[] = [];

  ngOnInit() {
    this.loadData();
  }

  loadData() {
    this.data = [...]; // Load from service
  }

  onCommandClick(args: any) {
    console.log('Command type:', args.commandType);
    console.log('Row data:', args.rowData);
    console.log('Button element:', args.target);
  }
}
```

### Command Type Checking

```typescript
import { Component, ViewChild, OnInit } from '@angular/core';
import { GridComponent } from '@syncfusion/ej2-angular-grids';

@Component({
  selector: 'app-command-check-grid',
  template: `
    <ejs-grid #grid [dataSource]="data" (commandClick)="onCommand($event)">
      <e-columns>
        <!-- columns with commands -->
      </e-columns>
    </ejs-grid>
  `
})
export class CommandCheckGridComponent implements OnInit {
  @ViewChild('grid') gridInstance: GridComponent;
  data: any[] = [];

  ngOnInit() {
    this.loadData();
  }

  loadData() {
    this.data = [...]; // Load from service
  }

  onCommand(args: any) {
    if (args.commandType === 'Edit') {
      console.log('Edit command:', args.rowData);
    } else if (args.commandType === 'Delete') {
      console.log('Delete command:', args.rowData);
    } else if (args.commandType === 'Custom') {
      console.log('Custom command:', args.commandType);
    }
  }
}
```

---

## Conditional Commands

### Show/Hide Based on Row Data

```typescript
import { Component, ViewChild } from '@angular/core';
import { GridComponent, EditService } from '@syncfusion/ej2-angular-grids';

@Component({
  selector: 'app-conditional-command',
  template: `
    <ejs-grid #grid [dataSource]="data" [editSettings]="editSettings">
      <e-columns>
        <e-column field="OrderID" headerText="Order ID" [isPrimaryKey]="true"></e-column>
        <e-column field="CustomerID" headerText="Customer"></e-column>
        <e-column field="Status" headerText="Status"></e-column>
        <e-column headerText="Actions" width="150" [cellTemplate]="'actionTemplate'">
        </e-column>
      </e-columns>
      <ng-template #actionTemplate let-data>
        <button class="e-btn e-small" (click)="editRow(data)" [style.margin-right.px]="5">
          Edit
        </button>
        <button 
          *ngIf="data.Status !== 'Completed"
          class="e-btn e-small e-danger" 
          (click)="deleteRow(data)">
          Delete
        </button>
        <button 
          *ngIf="data.Status === 'Pending"
          class="e-btn e-small e-success" 
          (click)="completeRow(data)">
          Complete
        </button>
      </ng-template>
    </ejs-grid>
  `,
  providers: [EditService]
})
export class ConditionalCommandComponent {
  @ViewChild('grid') gridComponent!: GridComponent;
  data: any[] = [];
  editSettings: any = { allowEditing: true, allowDeleting: true, mode: 'Dialog' };

  editRow(rowData: any): void {
    console.log('Edit row:', rowData);
  }

  deleteRow(rowData: any): void {
    if (confirm('Are you sure you want to delete this record?')) {
      this.gridComponent.deleteRecord();
    }
  }

  completeRow(rowData: any): void {
    console.log('Complete row:', rowData);
  }
```

### Role-Based Commands

```typescript
import { Component, ViewChild } from '@angular/core';
import { GridComponent, EditService } from '@syncfusion/ej2-angular-grids';

@Component({
  selector: 'app-role-command',
  template: `
    <ejs-grid #grid [dataSource]="data" [editSettings]="editSettings">
      <e-columns>
        <e-column field="OrderID" headerText="Order ID" [isPrimaryKey]="true"></e-column>
        <e-column field="CustomerID" headerText="Customer"></e-column>
        <e-column
          headerText="Actions"
          width="150"
          [commands]="getCommandsForRole()">
        </e-column>
      </e-columns>
    </ejs-grid>
  `,
  providers: [EditService]
})
export class RoleCommandComponent {
  @ViewChild('grid') gridComponent!: GridComponent;
  data: any[] = [];
  editSettings: any = { allowEditing: true, allowDeleting: true };
  userRole: string = 'user';

  ngOnInit() {
    this.getUserRole();
  }

  private getUserRole(): void {
    this.userRole = localStorage.getItem('userRole') || 'user';
  }

  getCommandsForRole(): any[] {
    if (this.userRole === 'admin') {
      return [
        { type: 'Edit', buttonOption: { cssClass: 'e-flat' } },
        { type: 'Delete', buttonOption: { cssClass: 'e-flat' } },
        { buttonOption: { content: 'Audit', cssClass: 'e-flat', click: this.viewAudit } }
      ];
    } else {
      return [
        { type: 'Edit', buttonOption: { cssClass: 'e-flat' } },
        { buttonOption: { content: 'View', cssClass: 'e-flat', click: this.viewDetails } }
      ];
    }
  }

  private viewAudit = (args: any): void => {
    console.log('Audit:', args.rowData);
  };

  private viewDetails = (args: any): void => {
    console.log('Details:', args.rowData);
  }
```

### Status-Based Commands

```typescript
import { Component, ViewChild, OnInit } from '@angular/core';
import { GridComponent } from '@syncfusion/ej2-angular-grids';

@Component({
  selector: 'app-status-command-grid',
  template: `
    <ejs-grid #grid [dataSource]="data">
      <e-columns>
        <!-- columns -->
        <e-column headerText="Actions" [commands]="getCommandsForStatus(row.Status)"></e-column>
      </e-columns>
    </ejs-grid>
  `
})
export class StatusCommandGridComponent implements OnInit {
  @ViewChild('grid') gridInstance: GridComponent;
  data: any[] = [];

  ngOnInit() {
    this.loadData();
  }

  loadData() {
    this.data = [...]; // Load from service
  }

  getCommandsForStatus(status: string) {
    const baseCommands = [{ type: 'Edit', buttonOption: { cssClass: 'e-flat' } }];

    if (status === 'Draft') {
      baseCommands.push({ type: 'Delete', buttonOption: { cssClass: 'e-flat' } });
    }

    if (status === 'Pending') {
      baseCommands.push({
        buttonOption: { content: 'Approve', cssClass: 'e-flat e-success', click: () => this.approveRow() }
      });
    }

    if (status === 'Completed') {
      baseCommands.push({
        buttonOption: { content: 'Archive', cssClass: 'e-flat e-info', click: () => this.archiveRow() }
      });
    }

    return baseCommands;
  }

  approveRow() {
    console.log('Row approved');
  }

  archiveRow() {
    console.log('Row archived');
  }
}
```

---

## Styling Commands

### Button Styling

```typescript
<e-column
  headerText="Actions"
  width="200"
  [commands]="[
    {
      type: 'Edit',
      buttonOption: {
        cssClass: 'e-flat e-outline',
        iconCss: 'e-icons e-edit'
      }
    },
    {
      type: 'Delete',
      buttonOption: {
        cssClass: 'e-flat e-danger',
        iconCss: 'e-icons e-delete'
      }
    },
    {
      buttonOption: {
        content: 'Archive',
        cssClass: 'e-flat e-warning',
        iconCss: 'e-icons e-archive'
      }
    }
  ]">
</e-column>
```

### CSS Classes

```css
/* Button sizes */
.e-btn.e-small { padding: 4px 8px; font-size: 12px; }
.e-btn.e-large { padding: 12px 16px; font-size: 16px; }

/* Button styles */
.e-btn.e-flat { background-color: transparent; border: 1px solid #ddd; }
.e-btn.e-outline { border: 2px solid currentColor; }
.e-btn.e-primary { background-color: #007bff; color: white; }
.e-btn.e-success { background-color: #28a745; color: white; }
.e-btn.e-danger { background-color: #dc3545; color: white; }
.e-btn.e-warning { background-color: #ffc107; color: black; }
.e-btn.e-info { background-color: #17a2b8; color: white; }

/* Hover effects */
.e-btn:hover { opacity: 0.8; cursor: pointer; }
.e-btn:disabled { opacity: 0.5; cursor: not-allowed; }
```

### Inline Styling

```typescript
import { Component, ViewChild } from '@angular/core';
import { GridComponent, EditService } from '@syncfusion/ej2-angular-grids';

@Component({
  selector: 'app-styled-command',
  template: `
    <ejs-grid #grid [dataSource]="data" [editSettings]="editSettings">
      <e-columns>
        <e-column field="OrderID" headerText="Order ID" [isPrimaryKey]="true"></e-column>
        <e-column field="CustomerID" headerText="Customer"></e-column>
        <e-column headerText="Actions" width="150" [cellTemplate]="'styledActionTemplate'">
        </e-column>
      </e-columns>
      <ng-template #styledActionTemplate let-data>
        <button 
          [ngStyle]="commandButtonStyle" 
          [style.background-color]="'#007bff'"
          (click)="editRow(data)">
          Edit
        </button>
        <button 
          [ngStyle]="commandButtonStyle" 
          [style.background-color]="'#dc3545'"
          (click)="deleteRow(data)">
          Delete
        </button>
      </ng-template>
    </ejs-grid>
  `,
  providers: [EditService]
})
export class StyledCommandComponent {
  @ViewChild('grid') gridComponent!: GridComponent;
  data: any[] = [];
  editSettings: any = { allowEditing: true, allowDeleting: true };

  commandButtonStyle = {
    padding: '6px 10px',
    fontSize: '12px',
    marginRight: '4px',
    border: 'none',
    borderRadius: '4px',
    cursor: 'pointer',
    color: 'white'
  };

  editRow(rowData: any): void {
    console.log('Edit row:', rowData);
  }

  deleteRow(rowData: any): void {
    if (confirm('Are you sure?')) {
      this.gridComponent.deleteRecord();
    }
  }
```

---

## Complete Example

```typescript
import { Component, ViewChild } from '@angular/core';
import { GridComponent, ColumnsDirective, ColumnDirective, Inject, Edit, Toolbar } from '@syncfusion/ej2-angular-grids';

@Component({
  selector: 'app-grid-commands',
  template: `
    <ejs-grid #gridInstance [dataSource]="orders" [editSettings]="editSettings" [toolbar]="toolbar" (commandClick)="onCommandClick($event)">
      <e-columns>
        <e-column field="OrderID" headerText="Order ID" width="100" isPrimaryKey="true"></e-column>
        <e-column field="CustomerID" headerText="Customer" width="120"></e-column>
        <e-column field="Freight" headerText="Freight" width="100" format="C2"></e-column>
        <e-column headerText="Actions" width="250" [commands]="commands"></e-column>
      </e-columns>
      <e-inject [services]="[Edit, Toolbar]"></e-inject>
    </ejs-grid>
  `
})
export class CommandComplexGridComponent implements OnInit {
  @ViewChild('grid') gridInstance: GridComponent;
  orders: any[] = [];
  editSettings: any = { mode: 'Dialog', allowEditing: true, allowDeleting: true };
  toolbar: string[] = [];
  commands: any[] = [];

  ngOnInit() {
    this.initializeCommands();
    this.loadOrders();
  }

  initializeCommands() {
    this.commands = [
      { 
        type: 'Edit', 
        buttonOption: { cssClass: 'e-flat', iconCss: 'e-icons e-edit' } 
      },
      { 
        buttonOption: { 
          content: 'Duplicate', 
          cssClass: 'e-flat e-info' 
        } 
      },
      { 
        buttonOption: { 
          content: 'Details', 
          cssClass: 'e-flat e-primary' 
        } 
      },
      { 
        type: 'Delete', 
        buttonOption: { cssClass: 'e-flat e-danger', iconCss: 'e-icons e-delete' } 
      }
    ];
  }

  onCommandClick(args: any) {
    switch (args.commandType) {
      case 'Edit':
        console.log('Editing:', args.rowData);
        break;
      case 'Delete':
        if (confirm('Are you sure you want to delete this record?')) {
          this.gridInstance.deleteRecord();
        }
        break;
      case 'Duplicate':
        this.onDuplicate(args);
        break;
      case 'Details':
        this.onViewDetails(args);
        break;
    }
  }

  onDuplicate(args: any) {
    const newData = { ...args.rowData };
    delete newData.OrderID;
    this.gridInstance.addRecord(newData);
  }

  onViewDetails(args: any) {
    alert(`Order ID: ${args.rowData.OrderID}\nCustomer: ${args.rowData.CustomerID}`);
  }

  loadOrders() {
    // Load orders from service
  }
}
```

---

## Best Practices

1. **Keep command column width fixed** (~150-250px)
2. **Order commands logically** (Edit → Save/Cancel, Delete at end)
3. **Use icons with text** for clarity
4. **Confirm destructive actions** (delete)
5. **Disable commands** when not applicable
6. **Show loading state** for async operations
7. **Provide keyboard shortcuts** (accessibility)
8. **Test with touch devices** (small button sizing)
9. **Group related commands** visually
10. **Provide tooltips** for clarity



