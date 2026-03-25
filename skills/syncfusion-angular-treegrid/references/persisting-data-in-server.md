---
name: Persisting Data in Server
description: 'Server-side Data Persistence in Syncfusion Angular TreeGrid - saving CRUD changes to backend, batch operations.'
---

# Persisting Data in Server

Server-side persistence ensures all CRUD operations are saved to the backend database with proper transaction handling and error management.

## Table of Contents
- [Basic Data Persistence](#basic-data-persistence)
- [Save Individual Changes](#save-individual-changes)
- [Batch Operations](#batch-operations)

## Basic Data Persistence

### Setup Server Communication

```typescript
import { Component, ViewChild } from '@angular/core';
import { TreeGridComponent } from '@syncfusion/ej2-angular-treegrid';
import { DataSourceChangedEventArgs } from '@syncfusion/ej2-grids';
import { HttpClient } from '@angular/common/http';

@Component({
  selector: 'app-treegrid',
  template: `
    <ejs-treegrid 
      #treegrid
      [dataSource]='data'
      [editSettings]='editSettings'
      (dataSourceChanged)='onDataSourceChanged($event)'>
      <e-columns>
        <e-column field='TaskID' headerText='ID' isPrimaryKey='true'></e-column>
        <e-column field='TaskName' headerText='Task'></e-column>
      </e-columns>
    </ejs-treegrid>
  `,
  providers: [HttpClientModule]
})
export class AppComponent {
  @ViewChild('treegrid') treegrid!: TreeGridComponent;
  
  public data: Object[] = [];
  private apiUrl = 'https://api.example.com/tasks';

  public editSettings = {
    mode: 'Normal',
    allowEditing: true,
    allowAdding: true,
    allowDeleting: true
  };

  constructor(private http: HttpClient) {}

  onDataSourceChanged(event: DataSourceChangedEventArgs): void {
    if (event.action === 'add') {
      this.saveNewRecord(event);
    } else if (event.action === 'edit') {
      this.updateRecord(event);
    } else if (event.requestType === 'delete') {
      this.deleteRecord(event);
    }
  }

  private saveNewRecord(event: any): void {
    this.http.post(this.apiUrl, event.data).subscribe({
      next: (response: any) => {
        event.data.TaskID = response.TaskID;  // Set server-generated ID
        event.endEdit();
      },
      error: (error) => {
        console.error('Failed to save record:', error);
        event.endEdit();
      }
    });
  }

  private updateRecord(event: any): void {
    const id = event.data.TaskID;
    this.http.put(`${this.apiUrl}/${id}`, event.data).subscribe({
      next: () => {
        event.endEdit();
      },
      error: (error) => {
        console.error('Failed to update record:', error);
        event.endEdit();
      }
    });
  }

  private deleteRecord(event: any): void {
    const id = event.data[0].TaskID;
    this.http.delete(`${this.apiUrl}/${id}`).subscribe({
      next: () => {
        event.endEdit();
      },
      error: (error) => {
        console.error('Failed to delete record:', error);
        event.endEdit();
      }
    });
  }
}
```

## Save Individual Changes

### Save on Cell Exit

```typescript
@Component({
  template: `
    <ejs-treegrid 
      [dataSource]='data'
      [editSettings]='{ mode: "Cell" }'
      (cellSave)='onCellSave($event)'>
    </ejs-treegrid>
  `
})
export class AppComponent {
  public data: Object[] = [];
  private apiUrl = 'https://api.example.com/tasks';

  onCellSave(args: any): void {
    console.log('Saving cell:', args);
    
    const updateData = {
      TaskID: args.rowData.TaskID,
      [args.column.field]: args.value
    };

    this.http.patch(`${this.apiUrl}/${args.rowData.TaskID}`, updateData)
      .subscribe({
        next: () => {
          console.log('Cell saved successfully');
        },
        error: (error) => {
          console.error('Save failed:', error);
          args.cancel = true;  // Revert cell change
        }
      });
  }
}
```

## Batch Operations

### Save Multiple Changes at Once

```typescript
import { Component, ViewChild } from '@angular/core';
import { TreeGridComponent } from '@syncfusion/ej2-angular-treegrid';

@Component({
  selector: 'app-treegrid',
  template: `
    <div class='toolbar'>
      <button (click)='saveBatch()'>Save All Changes</button>
      <button (click)='discardChanges()'>Discard changes</button>
    </div>
    
    <ejs-treegrid 
      #treegrid
      [dataSource]='data'
      [editSettings]='{ mode: "Batch" }'
      (beforeBatchSave)='onBeforeBatchSave($event)'>
    </ejs-treegrid>
  `
})
export class AppComponent {
  @ViewChild('treegrid') treegrid!: TreeGridComponent;
  
  public data: Object[] = [];
  private apiUrl = 'https://api.example.com/tasks';

  public editSettings = {
    mode: 'Batch',
    allowEditing: true,
    allowAdding: true,
    allowDeleting: true
  };

  onBeforeBatchSave(args: any): void {
    // Prevent automatic save
    args.cancel = true;
  }

  saveBatch(): void {
    const batchChanges = {
      addedRecords: this.treegrid.batchChanges?.addedRecords || [],
      changedRecords: this.treegrid.batchChanges?.changedRecords || [],
      deletedRecords: this.treegrid.batchChanges?.deletedRecords || []
    };

    this.http.post(`${this.apiUrl}/batch`, batchChanges).subscribe({
      next: (response: any) => {
        if (response.success) {
          this.treegrid.batchSave();
          alert('All changes saved successfully');
        } else {
          alert('Some changes failed to save');
        }
      },
      error: (error) => {
        console.error('Batch save failed:', error);
        alert('Failed to save changes');
      }
    });
  }

  discardChanges(): void {
    this.treegrid.batchCancel();
  }
}
```