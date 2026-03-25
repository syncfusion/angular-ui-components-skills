---
name: Editing
description: 'Editing in Syncfusion Angular TreeGrid - cell editing, inline editing, dialog editing, row editing, batch editing, and field validation.'
---

# Editing

Editing allows users to create, modify, and delete records with multiple editing modes and validation.

## Table of Contents
- [CRUD & Editing Rules](#crud--editing-rules)
- [Editing Modes](#editing-modes)
- [Configure Editing](#configure-editing)
- [Validation](#validation)
- [Events](#events)
- [Undo/Redo](#undoredo)
- [Optimistic Updates](#optimistic-updates)
- [Inline Error Display](#inline-error-display)
- [Cross-Field Validation](#cross-field-validation)

## CRUD & Editing Rules

### Rule 1: EditSettings is MANDATORY for Editing
**Severity**: 🔴 CRITICAL - Editing won't work without configuration

**Requirement**:
```typescript
// ✅ REQUIRED - Must define editSettings
public editSettings = {
  mode: 'Cell',                    // Required: 'Cell', 'Dialog', 'Inline', 'Batch'
  allowEditing: true,              // Required: Enable editing
  allowAdding: true,               // Optional: Enable adding rows
  allowDeleting: true              // Optional: Enable deleting rows
};

<ejs-treegrid 
  [editSettings]='editSettings'
  [primaryKey]='TaskID'>           // Also need primary key
</ejs-treegrid>

// ❌ WRONG - No editSettings = No editing possible
<ejs-treegrid>
  <!-- No editing functionality -->
</ejs-treegrid>
```

**Additional Requirements**:
```typescript
// ✅ MUST inject EditService for any editing mode
import { EditService } from '@syncfusion/ej2-angular-treegrid';

@Component({
  providers: [EditService]  // REQUIRED
})
```

**Edit Mode Rules**:
```typescript
// Cell Mode - Edit individual cells
editSettings: { mode: 'Cell', allowEditing: true }

// Dialog Mode - Edit in popup dialog
editSettings: { mode: 'Dialog', allowEditing: true }

// Inline Mode - Edit full row inline
editSettings: { mode: 'Inline', allowEditing: true }

// Batch Mode - Edit multiple rows, save all at once
editSettings: { mode: 'Batch', allowEditing: true }
```

---

### Rule 2: EditService Module Injection is MANDATORY for Editing
**Severity**: 🔴 CRITICAL - Editing features won't initialize without service

**Requirement**:
```typescript
// ✅ REQUIRED - EditService must be injected
import { EditService } from '@syncfusion/ej2-angular-treegrid';

@Component({
  selector: 'app-treegrid',
  template: `...`,
  providers: [EditService]  // MANDATORY for editing
})
export class AppComponent {}

// ❌ WRONG - Missing EditService
@Component({
  providers: []  // Service not injected = Editing fails
})
```

---

### Rule 3: Column EditType Must Match Data Type
**Severity**: 🟠 IMPORTANT - Validation fails with mismatched editTypes

**Requirement**:
```typescript
// ✅ CORRECT - EditType matches data type
<e-columns>
  <e-column field='TaskName' 
    headerText='Task' 
    editType='stringedit'></e-column>
    
  <e-column field='Duration' 
    headerText='Duration' 
    editType='numericedit'
    [validationRules]='{ required: true, min: 1, max: 1000 }'></e-column>
    
  <e-column field='Priority' 
    headerText='Priority' 
    editType='dropdownedit'></e-column>
    
  <e-column field='StartDate' 
    headerText='Start Date' 
    editType='datepickeredit'
    format='yMd'></e-column>
    
  <e-column field='IsCompleted' 
    headerText='Completed' 
    editType='booleanedit'></e-column>
</e-columns>

// ❌ WRONG - EditType mismatch
<e-column field='Duration' 
  headerText='Duration' 
  editType='TextBox'>  <!-- Should be NumericTextBox -->
</e-column>
```

**Valid EditTypes**:
| EditType | Best For | Validation |
|----------|----------|------------|
| stringedit | String fields | max length |
| numericedit | Numbers | min, max, decimals |
| dropdownedit | Fixed options | required |
| datepickeredit | Dates | date range |
| booleanedit | Booleans | true/false |
| datetimepickeredit| |Date and Time| |date and time range|

---

### Rule 4: Primary Key (isPrimaryKey) is MANDATORY for CRUD Operations
**Severity**: 🔴 CRITICAL - Editing/Deleting fails silently without this

**Requirement**:
```typescript
// ✅ REQUIRED - Exactly ONE column must have isPrimaryKey='true'
<e-columns>
  <e-column field='TaskID' headerText='ID' isPrimaryKey='true'></e-column>
  <e-column field='TaskName' headerText='Task'></e-column>
</e-columns>

// ❌ WRONG - No primary key = CRUD operations fail silently
<e-columns>
  <e-column field='TaskID' headerText='ID'></e-column>
  <e-column field='TaskName' headerText='Task'></e-column>
</e-columns>
```

**Rules**:
- ✅ Only ONE primary key allowed per grid
- ✅ Primary key value must be UNIQUE for each row
- ✅ Primary key must not be NULL/undefined
- ✅ Must match a field in data source
- ❌ Do NOT mark multiple columns as primary key
- ❌ Do NOT use composite/multi-column primary keys

**Impact Without This**:
```typescript
// What fails without isPrimaryKey:
- Edit operations (beginEdit fails silently)
- Delete operations (no row deleted)
- Update operations (data not persisted)
- Row selection preservation
```

---
## Editing Modes

### Cell Editing

```typescript
import { Component } from '@angular/core';
import { EditService } from '@syncfusion/ej2-angular-treegrid';

@Component({
  selector: 'app-treegrid',
  template: `
    <ejs-treegrid 
      [dataSource]='data'
      [childMapping]='childMapping'
      [editSettings]='editSettings'>
      <e-columns>
        <e-column field='TaskID' headerText='Task ID' width='90' isPrimaryKey='true'></e-column>
        <e-column field='TaskName' headerText='Task Name' width='200'></e-column>
      </e-columns>
    </ejs-treegrid>
  `,
  providers: [EditService]
})
export class AppComponent {
  public data: Object[] = [];
  public childMapping: string = 'subtasks';

  public editSettings = {
    mode: 'Cell'
  };
}
```

### Dialog Editing

```typescript
public editSettings = {
  mode: 'Dialog'
};
```

### Row Editing

```typescript
public editSettings = {
  mode: 'Row'
};
```

### Batch Editing

```typescript
public editSettings = {
  mode: 'Batch'
};
```

## Configure Editing

### Enable CRUD Operations

```typescript
public editSettings = {
  mode: 'Cell',
  allowEditing: true,
  allowDeleting: true,
  allowAdding: true
};
```

### Column Editor Type

```typescript
<e-column field='Duration' headerText='Duration' type='number' editType='numerictextbox' width='100'></e-column>
<e-column field='StartDate' headerText='Start Date' type='date' editType='datepicker' width='130'></e-column>
<e-column field='Status' headerText='Status' editType='dropdownlist' width='100'></e-column>
```

## Validation

### Field Validation

```typescript
<e-column 
  field='TaskName' 
  headerText='Task Name' 
  width='200'
  validationRules='{ required: true }'>
</e-column>

<e-column 
  field='Duration' 
  headerText='Duration' 
  type='number'
  validationRules='{ required: true, min: 1, max: 100 }'>
</e-column>
```

## Events

### Edit Events

```typescript
<ejs-treegrid 
  (actionBegin)='onActionBegin($event)'
  (actionComplete)='onActionComplete($event)'
  (actionFailure)='onActionFailure($event)'>
  <!-- columns -->
</ejs-treegrid>
```

```typescript
onActionBegin(args: any) {
  if (args.requestType === 'save') {
    console.log('Saving record:', args.data);
  }
}

onActionComplete(args: any) {
  if (args.requestType === 'save') {
    console.log('Record saved successfully');
  }
}

onActionFailure(args: any) {
  console.error('Edit operation failed:', args);
}
```

## Undo/Redo

### Implement Undo/Redo Stack

```typescript
import { Component, ViewChild } from '@angular/core';
import { TreeGridComponent } from '@syncfusion/ej2-angular-treegrid';

interface EditHistory {
  action: 'add' | 'edit' | 'delete';
  originalData: Object;
  modifiedData: Object;
  timestamp: number;
}

@Component({
  selector: 'app-treegrid',
  template: `
    <div class='toolbar'>
      <button (click)='undo()' [disabled]='!canUndo'>Undo</button>
      <button (click)='redo()' [disabled]='!canRedo'>Redo</button>
    </div>
    <ejs-treegrid 
      #treegrid
      [dataSource]='data'
      (actionComplete)='onActionComplete($event)'>
    </ejs-treegrid>
  `
})
export class AppComponent {
  @ViewChild('treegrid') treegrid!: TreeGridComponent;
  
  public data: Object[] = [];
  private undoStack: EditHistory[] = [];
  private redoStack: EditHistory[] = [];

  get canUndo(): boolean {
    return this.undoStack.length > 0;
  }

  get canRedo(): boolean {
    return this.redoStack.length > 0;
  }

  onActionComplete(args: any): void {
    if (args.requestType === 'save' || args.requestType === 'delete') {
      const history: EditHistory = {
        action: args.requestType === 'save' ? 'edit' : 'delete',
        originalData: { ...args.previous },
        modifiedData: { ...args.data },
        timestamp: Date.now()
      };
      this.undoStack.push(history);
      this.redoStack = [];
    }
  }

  undo(): void {
    if (this.undoStack.length === 0) return;
    
    const history = this.undoStack.pop()!;
    this.redoStack.push(history);
    
    const index = this.data.findIndex(
      item => item.TaskID === history.originalData.TaskID
    );
    if (index !== -1) {
      this.data[index] = { ...history.originalData };
      this.data = [...this.data];
    }
  }

  redo(): void {
    if (this.redoStack.length === 0) return;
    
    const history = this.redoStack.pop()!;
    this.undoStack.push(history);
    
    const index = this.data.findIndex(
      item => item.TaskID === history.modifiedData.TaskID
    );
    if (index !== -1) {
      this.data[index] = { ...history.modifiedData };
      this.data = [...this.data];
    }
  }
}
```

## Optimistic Updates

### Apply Changes Before Server Confirmation

```typescript
import { Component, ViewChild } from '@angular/core';
import { TreeGridComponent } from '@syncfusion/ej2-angular-treegrid';
import { HttpClient } from '@angular/common/http';

@Component({
  selector: 'app-treegrid',
  template: `
    <ejs-treegrid 
      #treegrid
      [dataSource]='data'
      [editSettings]='editSettings'
      (beforeBatchSave)='onBeforeBatchSave($event)'
      (actionFailure)='onActionFailure($event)'>
    </ejs-treegrid>
  `
})
export class AppComponent {
  @ViewChild('treegrid') treegrid!: TreeGridComponent;
  
  public data: Object[] = [];
  public editSettings = { mode: 'Batch' };
  private originalData: Object[] = [];

  constructor(private http: HttpClient) {}

  onBeforeBatchSave(args: any): void {
    this.originalData = JSON.parse(JSON.stringify(this.data));
    args.cancel = false;
    this.saveChangesToServer(args.changedRecords);
  }

  private saveChangesToServer(changes: any[]): void {
    this.http.post('/api/tasks/batch', changes).subscribe({
      next: (response) => {
        console.log('Changes saved successfully');
      },
      error: (error) => {
        console.error('Save failed, reverting changes');
        this.data = JSON.parse(JSON.stringify(this.originalData));
        this.treegrid.refresh();
      }
    });
  }

  onActionFailure(args: any): void {
    console.error('Operation failed:', args);
  }
}
```

## Inline Error Display

### Show Validation Errors in Cells

```typescript
import { Component, ViewChild } from '@angular/core';
import { TreeGridComponent } from '@syncfusion/ej2-angular-treegrid';

@Component({
  selector: 'app-treegrid',
  template: `
    <ejs-treegrid 
      #treegrid
      [dataSource]='data'
      [editSettings]='editSettings'
      (actionComplete)='onActionComplete($event)'>
      <e-columns>
        <e-column 
          field='TaskName' 
          headerText='Task Name'
          validationRules='{ required: true, minLength: 3 }'>
        </e-column>
        <e-column 
          field='Duration' 
          headerText='Duration'
          type='number'
          validationRules='{ required: true, min: 1 }'>
        </e-column>
      </e-columns>
    </ejs-treegrid>
  `
})
export class AppComponent {
  @ViewChild('treegrid') treegrid!: TreeGridComponent;
  
  public data: Object[] = [];
  public editSettings = { mode: 'Cell', allowEditing: true };

  onActionComplete(args: any): void {
    if (args.requestType === 'save') {
      const validationResult = this.validateRecord(args.data);
      if (!validationResult.isValid) {
        args.cancel = true;
        this.showInlineErrors(args.data, validationResult.errors);
      }
    }
  }

  private validateRecord(record: any): { isValid: boolean; errors: string[] } {
    const errors: string[] = [];
    
    if (!record.TaskName || record.TaskName.trim().length < 3) {
      errors.push('Task name must be at least 3 characters');
    }
    if (!record.Duration || record.Duration < 1) {
      errors.push('Duration must be at least 1');
    }
    
    return {
      isValid: errors.length === 0,
      errors
    };
  }

  private showInlineErrors(record: any, errors: string[]): void {
    const errorMessage = errors.join(', ');
    console.warn('Validation errors:', errorMessage);
  }

  onCellSelected(args: any): void {
    const prevErrorCell = document.querySelector('.cell-error');
    if (prevErrorCell) {
      prevErrorCell.classList.remove('cell-error');
    }
  }
}
```

## Cross-Field Validation

### Validate Multiple Fields Together

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-treegrid',
  template: `
    <ejs-treegrid 
      [dataSource]='data'
      [editSettings]='editSettings'
      (actionComplete)='onActionComplete($event)'>
      <e-columns>
        <e-column field='StartDate' headerText='Start Date' type='date'></e-column>
        <e-column field='EndDate' headerText='End Date' type='date'></e-column>
        <e-column field='Duration' headerText='Duration' type='number'></e-column>
      </e-columns>
    </ejs-treegrid>
  `
})
export class AppComponent {
  public data: Object[] = [];
  public editSettings = { mode: 'Cell', allowEditing: true };

  onActionComplete(args: any): void {
    if (args.requestType === 'save') {
      const validation = this.validateCrossFields(args.data);
      if (!validation.valid) {
        args.cancel = true;
        alert(validation.message);
      }
    }
  }

  private validateCrossFields(record: any): { valid: boolean; message: string } {
    if (!record.StartDate || !record.EndDate) {
      return { valid: true, message: '' };
    }

    const startDate = new Date(record.StartDate);
    const endDate = new Date(record.EndDate);

    if (endDate <= startDate) {
      return {
        valid: false,
        message: 'End date must be after start date'
      };
    }

    const duration = Math.floor(
      (endDate.getTime() - startDate.getTime()) / (1000 * 60 * 60 * 24)
    );

    if (record.Duration && record.Duration !== duration) {
      return {
        valid: false,
        message: `Duration (${record.Duration}) does not match the date range (${duration} days)`
      };
    }

    return { valid: true, message: '' };
  }
}
```
