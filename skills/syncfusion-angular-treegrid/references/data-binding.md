---
name: Data-binding
description: 'Data binding in Syncfusion Angular TreeGrid - local data arrays, remote data sources, ORM integration, custom adaptors, and dynamic data assignment.'
---

# Data Binding

Data binding connects the TreeGrid to various data sources including local arrays, remote APIs, and ORMs.

## Table of Contents
- [Local Data Binding](#local-data-binding)
- [Remote Data Binding](#remote-data-binding)
- [ORM Integration](#orm-integration)
- [Custom Adaptor](#custom-adaptor)
- [Error Handling](#error-handling)
- [Retry Logic](#retry-logic)
- [Offline Sync](#offline-sync)
- [Change Detection Optimization](#change-detection-optimization)

## Local Data Binding

### Bind Local Array

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-treegrid',
  template: `
    <ejs-treegrid 
      [dataSource]='taskData'
      [childMapping]='childMapping'>
      <e-columns>
        <e-column field='TaskID' headerText='Task ID' width='90'></e-column>
        <e-column field='TaskName' headerText='Task Name' width='150'></e-column>
        <e-column field='Duration' headerText='Duration' width='100'></e-column>
      </e-columns>
    </ejs-treegrid>
  `
})
export class AppComponent {
  public childMapping: string = 'subtasks';
  
  public taskData: Object[] = [
    {
      TaskID: 1,
      TaskName: 'Planning',
      Duration: 8,
      subtasks: [
        { TaskID: 2, TaskName: 'Scope', Duration: 4 },
        { TaskID: 3, TaskName: 'Budget', Duration: 4 }
      ]
    }
  ];
}
```

### Dynamic Data Assignment

```typescript
setDataSource(data: Object[]) {
  this.treegrid.dataSource = data;
  this.treegrid.refresh();
}
```

## Remote Data Binding

### UrlAdaptor

```typescript
import { DataManager, UrlAdaptor } from '@syncfusion/ej2-data';

export class AppComponent {
  public dataManager = new DataManager({
    url: 'https://api.example.com/tasks',
    adaptor: new UrlAdaptor()
  });
}
```

### With DataManager

```typescript
<ejs-treegrid 
  [dataSource]='dataManager'
  idMapping='TaskID'
  parentIdMapping='ParentItem'
  hasChildMapping='isParent' 
  [allowPaging]='true'
  [pageSettings]='{ pageSize: 10 }'>
  <!-- columns -->
</ejs-treegrid>
```

## ORM Integration

### Entity Framework Core

```typescript
public dataManager = new DataManager({
  url: 'https://api.example.com/odata/tasks',
  adaptor: new ODataV4Adaptor(),
  crossDomain: true
});
```

## Custom Adaptor

### Create Custom Adaptor

```typescript
import { DataManager, Adaptor, Query } from '@syncfusion/ej2-data';

class CustomAdaptor extends Adaptor {
  public read(dm: DataManager, query: Query): Promise<Object> {
    return fetch('https://api.example.com/data')
      .then(res => res.json());
  }

  public insert(dm: DataManager, data: Object): Promise<Object> {
    return fetch('https://api.example.com/data', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(data)
    }).then(res => res.json());
  }
}
```

## Error Handling

### Handle Data Binding Errors

```typescript
import { Component, ViewChild, OnInit } from '@angular/core';
import { TreeGridComponent, DataManager, UrlAdaptor } from '@syncfusion/ej2-angular-treegrid';

@Component({
  selector: 'app-treegrid',
  template: `
    <div *ngIf='errorMessage' class='error-message'>
      {{ errorMessage }}
    </div>
    <ejs-treegrid 
      #treegrid
      [dataSource]='dataManager'
      (actionFailure)='onActionFailure($event)'>
    </ejs-treegrid>
  `
})
export class AppComponent implements OnInit {
  @ViewChild('treegrid') treegrid!: TreeGridComponent;
  
  public dataManager: DataManager = new DataManager({
    url: 'https://api.example.com/tasks',
    adaptor: new UrlAdaptor()
  });
  
  public errorMessage: string = '';

  ngOnInit(): void {
    // Prevent error alerts from automatic display
    this.treegrid.grid?.addEventListener(
      'actionFailure',
      this.onActionFailure.bind(this)
    );
  }

  onActionFailure(args: any): void {
    if (args.error) {
      this.errorMessage = args.error.statusText || 'Data loading failed';
      console.error('Data Error:', args.error);
    }
  }
}
```

## Retry Logic

### Automatic Retry with Exponential Backoff

```typescript
import { Injectable } from '@angular/core';
import { HttpClient, HttpErrorResponse } from '@angular/common/http';
import { of, throwError } from 'rxjs';
import { catchError, retry, retryWhen, delay, take } from 'rxjs/operators';

@Injectable({ providedIn: 'root' })
export class DataService {
  constructor(private http: HttpClient) {}

  loadTreeData(url: string) {
    const maxRetries = 3;
    const delayMs = 1000;

    return this.http.get(url).pipe(
      retryWhen(errors =>
        errors.pipe(
          delay(delayMs),
          take(maxRetries),
          catchError(err => {
            console.error('Max retries reached', err);
            return throwError(err);
          })
        )
      ),
      catchError((error: HttpErrorResponse) => {
        console.error('Failed to load data:', error.message);
        return throwError(() => new Error('Unable to load data'));
      })
    );
  }
}
```

### In Component

```typescript
export class AppComponent implements OnInit {
  @ViewChild('treegrid') treegrid!: TreeGridComponent;
  
  public data: Object[] = [];

  constructor(private dataService: DataService) {}

  ngOnInit(): void {
    this.dataService
      .loadTreeData('https://api.example.com/tasks')
      .subscribe({
        next: (result: any) => {
          this.data = result.data;
          this.treegrid.dataSource = this.data;
        },
        error: (error) => {
          console.error('Failed to load data after retries:', error);
          // Fall back to cached or local data
        }
      });
  }
}
```

## Offline Sync

### Detect Offline and Sync When Online

```typescript
import { Component, OnInit } from '@angular/core';

interface OfflineSyncData {
  operation: 'add' | 'edit' | 'delete';
  data: Object;
  timestamp: number;
}

@Component({
  selector: 'app-treegrid',
  template: `
    <div *ngIf='!isOnline' class='offline-banner'>
      You are offline. Changes will sync when online.
    </div>
    <ejs-treegrid 
      [dataSource]='data'
      (actionComplete)='onActionComplete($event)'>
    </ejs-treegrid>
  `
})
export class AppComponent implements OnInit {
  public isOnline: boolean = navigator.onLine;
  public data: Object[] = [];
  private syncQueue: OfflineSyncData[] = [];

  ngOnInit(): void {
    window.addEventListener('online', () => this.syncData());
    window.addEventListener('offline', () => {
      this.isOnline = false;
    });
  }

  onActionComplete(args: any): void {
    if (!this.isOnline) {
      // Queue the operation for later sync
      this.syncQueue.push({
        operation: args.requestType,
        data: args.data,
        timestamp: Date.now()
      });
      // Save to localStorage
      localStorage.setItem('syncQueue', JSON.stringify(this.syncQueue));
    }
  }

  private syncData(): void {
    this.isOnline = true;
    const queuedData = localStorage.getItem('syncQueue');
    
    if (queuedData) {
      this.syncQueue = JSON.parse(queuedData);
      // Process each queued operation
      this.syncQueue.forEach(item => {
        this.sendToServer(item);
      });
      localStorage.removeItem('syncQueue');
    }
  }

  private sendToServer(item: OfflineSyncData): void {
    // Send to API based on operation type
    console.log('Syncing:', item);
  }
}
```

## Change Detection Optimization

### Use OnPush Strategy for Large Datasets

```typescript
import { Component, ChangeDetectionStrategy, Input, OnInit } from '@angular/core';
import { DataManager, UrlAdaptor } from '@syncfusion/ej2-data';

@Component({
  selector: 'app-treegrid',
  template: `
    <ejs-treegrid 
      [dataSource]='dataManager'
      [childMapping]='childMapping'
      [allowVirtualization]='true'>
      <e-columns>
        <e-column field='TaskID' headerText='Task ID' width='90'></e-column>
        <e-column field='TaskName' headerText='Task Name' width='200'></e-column>
      </e-columns>
    </ejs-treegrid>
  `,
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class AppComponent implements OnInit {
  @Input() dataSource: string = 'https://api.example.com/tasks';
  
  public dataManager!: DataManager;
  public childMapping: string = 'Children';

  ngOnInit(): void {
    this.dataManager = new DataManager({
      url: this.dataSource,
      adaptor: new UrlAdaptor(),
      // Enable caching to reduce API calls
      enableCaching: true,
      cachingPageSize: 50
    });
  }
}
```

### Batch Data Updates

```typescript
// Instead of updating individual records, batch updates
updateMultipleRecords(changes: Object[]): void {
  // Apply all changes at once to minimize change detection runs
  const updatedData = [...this.data];
  changes.forEach(change => {
    const index = updatedData.findIndex(item => item.TaskID === change.TaskID);
    if (index !== -1) {
      updatedData[index] = { ...updatedData[index], ...change };
    }
  });
  // Single assignment triggers change detection once
  this.data = updatedData;
}
```
