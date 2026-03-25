---
name: Getting Started Guide
description: 'Getting Started with Syncfusion Angular TreeGrid - installation, setup, module configuration, first component, and basic examples.'
---

# Getting Started Guide

Quick start guide to set up Syncfusion Angular TreeGrid in your Angular project.

## Table of Contents
- [Installation](#installation)
- [Module Setup](#module-setup)
- [Create First Component](#create-first-component)
- [Basic Configuration](#basic-configuration)
- [Data Binding Basics](#data-binding-basics)
- [Common Issues](#common-issues)

## Installation

### Step 1: Install Syncfusion Packages

```bash
npm install @syncfusion/ej2-angular-treegrid --save
npm install @syncfusion/ej2-base --save
npm install @syncfusion/ej2-Angular-grids --save
```

### Step 2: Include CSS

Add CSS files to your `styles.css` or `angular.json`:

```css
/* styles.css */
@import "@syncfusion/ej2-base/styles/material.css";
@import "@syncfusion/ej2-buttons/styles/material.css";
@import "@syncfusion/ej2-calendars/styles/material.css";
@import "@syncfusion/ej2-dropdowns/styles/material.css";
@import "@syncfusion/ej2-inputs/styles/material.css";
@import "@syncfusion/ej2-popups/styles/material.css";
@import "@syncfusion/ej2-grids/styles/material.css";
@import "@syncfusion/ej2-angular-treegrid/styles/material.css";
```

Or update `angular.json`:

```json
{
  "projects": {
    "your-app": {
      "architect": {
        "build": {
          "options": {
            "styles": [
              "@syncfusion/ej2-base/styles/material.css",
              "@syncfusion/ej2-buttons/styles/material.css",
              "@syncfusion/ej2-grids/styles/material.css",
              "@syncfusion/ej2-angular-treegrid/styles/material.css"
            ]
          }
        }
      }
    }
  }
}
```

### Step 3: Register License Key (Optional but Recommended)

For licensed versions, register your license key in `main.ts`:

```typescript
import { registerLicense } from '@syncfusion/ej2-base';

registerLicense('YOUR_SYNCFUSION_LICENSE_KEY');

bootstrapApplication(AppComponent);
```

## Module Setup

### Import TreeGrid Module

```typescript
import { Component } from '@angular/core';
import { TreeGridModule } from '@syncfusion/ej2-angular-treegrid';

@Component({
  selector: 'app-root',
  standalone: true,
  imports: [TreeGridModule],
  template: `
    <ejs-treegrid [dataSource]='data'>
      <e-columns>
        <e-column field='TaskID' headerText='Task ID' width='90'></e-column>
        <e-column field='TaskName' headerText='Task Name' width='150'></e-column>
      </e-columns>
    </ejs-treegrid>
  `
})
export class AppComponent {
  public data: Object[] = [];
}
```

### Import in NgModule (Traditional)

```typescript
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { TreeGridModule } from '@syncfusion/ej2-angular-treegrid';

@NgModule({
  declarations: [AppComponent],
  imports: [BrowserModule, TreeGridModule],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

## Create First Component

### Basic TreeGrid Component

```typescript
import { Component, ViewChild } from '@angular/core';
import { TreeGridComponent } from '@syncfusion/ej2-angular-treegrid';

@Component({
  selector: 'app-treegrid',
  template: `
    <ejs-treegrid 
      #treegrid
      [dataSource]='data'
      [childMapping]='childMapping'
      [enablePersistence]='true'>
      <e-columns>
        <e-column field='TaskID' headerText='Task ID' width='90' isPrimaryKey='true'></e-column>
        <e-column field='TaskName' headerText='Task Name' width='150'></e-column>
        <e-column field='Duration' headerText='Duration' width='100'></e-column>
        <e-column field='Progress' headerText='Progress' width='100'></e-column>
      </e-columns>
    </ejs-treegrid>
  `,
  styles: [`
    ::ng-deep .e-treegrid {
      font-family: 'Segoe UI', sans-serif;
    }
  `]
})
export class AppComponent {
  @ViewChild('treegrid') treegrid!: TreeGridComponent;

  public childMapping: string = 'subtasks';
  
  public data: Object[] = [
    {
      TaskID: 1,
      TaskName: 'Planning',
      Duration: 8,
      Progress: 100,
      subtasks: [
        { TaskID: 2, TaskName: 'Requirements', Duration: 4, Progress: 100 },
        { TaskID: 3, TaskName: 'Design', Duration: 4, Progress: 100 }
      ]
    },
    {
      TaskID: 4,
      TaskName: 'Development',
      Duration: 16,
      Progress: 75,
      subtasks: [
        { TaskID: 5, TaskName: 'Backend', Duration: 10, Progress: 80 },
        { TaskID: 6, TaskName: 'Frontend', Duration: 6, Progress: 70 }
      ]
    }
  ];
}
```

## Basic Configuration

### Enable Common Features

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-treegrid',
  template: `
    <ejs-treegrid 
      [dataSource]='data'
      [childMapping]='childMapping'
      [allowPaging]='true'
      [allowSorting]='true'
      [allowFiltering]='true'
      [pageSettings]='pageSettings'
      [editSettings]='editSettings'>
      <e-columns>
        <e-column field='TaskID' headerText='Task ID' width='90' isPrimaryKey='true'></e-column>
        <e-column field='TaskName' headerText='Task Name' width='150'></e-column>
        <e-column field='Duration' headerText='Duration' width='100'></e-column>
        <e-column field='Progress' headerText='Progress' width='100'></e-column>
      </e-columns>
    </ejs-treegrid>
  `
})
export class AppComponent {
  public childMapping: string = 'subtasks';
  
  public pageSettings = {
    pageSize: 20
  };

  public editSettings = {
    allowEditing: true,
    allowAdding: true,
    allowDeleting: true,
    mode: 'Cell'
  };

  public data: Object[] = [
    // ... data
  ];
}
```

### Inject Required Services

For advanced features, inject required services:

```typescript
import { Component } from '@angular/core';
import { 
  PageService, 
  SortService, 
  FilterService, 
  EditService,
  ExcelExportService,
  PdfExportService,
  PrintService
} from '@syncfusion/ej2-angular-treegrid';

@Component({
  selector: 'app-treegrid',
  template: `...`,
  providers: [
    PageService,
    SortService,
    FilterService,
    EditService,
    ExcelExportService,
    PdfExportService,
    PrintService
  ]
})
export class AppComponent {
  // ...
}
```

## Data Binding Basics

### Local Data Array

```typescript
public data: Object[] = [
  { TaskID: 1, TaskName: 'Task 1', Duration: 8 },
  { TaskID: 2, TaskName: 'Task 2', Duration: 10 }
];
```

### Hierarchical Data with Child Mapping

```typescript
public data: Object[] = [
  {
    TaskID: 1,
    TaskName: 'Parent Task',
    Duration: 8,
    subtasks: [
      { TaskID: 2, TaskName: 'Child Task 1', Duration: 4 },
      { TaskID: 3, TaskName: 'Child Task 2', Duration: 4 }
    ]
  }
];

public childMapping: string = 'subtasks';
```

### Remote Data with DataManager

```typescript
import { DataManager, UrlAdaptor } from '@syncfusion/ej2-data';

@Component({
  template: `
    <ejs-treegrid 
      [dataSource]='dataManager'
      [childMapping]='childMapping'>
    </ejs-treegrid>
  `
})
export class AppComponent {
  public childMapping: string = 'subtasks';

  public dataManager = new DataManager({
    url: 'https://api.example.com/tasks',
    adaptor: new UrlAdaptor()
  });
}
```

### Dynamic Data Loading

```typescript
import { Component, OnInit } from '@angular/core';
import { HttpClient } from '@angular/common/http';

@Component({
  selector: 'app-treegrid',
  template: `
    <div *ngIf='loading'>Loading...</div>
    <ejs-treegrid *ngIf='!loading' [dataSource]='data'>
      <!-- columns -->
    </ejs-treegrid>
  `
})
export class AppComponent implements OnInit {
  public data: Object[] = [];
  public loading: boolean = true;

  constructor(private http: HttpClient) {}

  ngOnInit() {
    this.loadData();
  }

  private loadData(): void {
    this.http.get('https://api.example.com/tasks').subscribe({
      next: (response: any) => {
        this.data = response;
        this.loading = false;
      },
      error: (error) => {
        console.error('Failed to load data:', error);
        this.loading = false;
      }
    });
  }
}
```

## Common Issues

### Issue: Module Not Found

**Error:** `Cannot find module '@syncfusion/ej2-angular-treegrid'`

**Solution:**
```bash
npm install @syncfusion/ej2-angular-treegrid --save
npm install @syncfusion/ej2-angular-grids --save
npm install @syncfusion/ej2-base --save
```

### Issue: Styles Not Applied

**Error:** TreeGrid renders but looks unstyled

**Solution:** Ensure CSS imports are in your `styles.css` or `angular.json`

```css
@import "@syncfusion/ej2-base/styles/material.css";
@import "@syncfusion/ej2-angular-treegrid/styles/material.css";
```

### Issue: TreeGrid Not Rendering

**Error:** Component renders but no content shows

**Solution:** Ensure data is provided and columns are defined

```typescript
public data: Object[] = [
  { TaskID: 1, TaskName: 'Task 1' }
];

// In template:
<e-column field='TaskID' headerText='Task ID'></e-column>
<e-column field='TaskName' headerText='Task Name'></e-column>
```

### Issue: Child Records Not Expanding

**Error:** Hierarchical data doesn't show expand arrows

**Solution:** Verify `childMapping` matches your data property

```typescript
// Correct mapping
public childMapping: string = 'subtasks'; // Must match data property

// Data structure
public data = [{
  TaskID: 1,
  subtasks: [  // Property name must match childMapping
    { TaskID: 2 }
  ]
}];
```

## Next Steps

1. **Read:** [modules.md](modules.md) - Learn required module imports
2. **Read:** [data-binding.md](data-binding.md) - Master data binding techniques
3. **Read:** [editing.md](editing.md) - Add editing capabilities
4. **Read:** [filtering.md](filtering.md) - Implement filtering
5. **Read:** [sorting.md](sorting.md) - Add sorting functionality
6. **Read:** [paging.md](paging.md) - Configure pagination
7. **Explore:** [advanced-features.md](advanced-features.md) - Discover advanced capabilities

## Quick Reference

| Feature | Enable |
|---------|--------|
| Paging | `[allowPaging]='true'` |
| Sorting | `[allowSorting]='true'` |
| Filtering | `[allowFiltering]='true'` |
| Editing | `[editSettings]='editSettings'` |
| Selection | Built-in by default |
| Virtualization | `[enableVirtualization]='true'` |
| Export | Inject ExcelExportService, PdfExportService |

## Resources

- [Syncfusion Documentation](https://www.syncfusion.com/angular-components/angular-tree-grid)
- [GitHub Examples](https://github.com/syncfusion/ej2-angular-samples)
- [Stack Overflow Tag](https://stackoverflow.com/questions/tagged/syncfusion)
- [Support Forum](https://www.syncfusion.com/forums/angular-treegrid)
