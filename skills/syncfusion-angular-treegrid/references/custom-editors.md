---
name: Custom-editors
description: 'Custom Editors in Syncfusion Angular TreeGrid - create custom edit controls, editor components, validation integration, and lifecycle hooks.'
---

# Custom Editors

Create custom editor components for complex data editing requirements beyond standard edit types.

## Table of Contents
- [Custom Editor Components](#custom-editor-components)
- [Editor Lifecycle](#editor-lifecycle)
- [Validation in Custom Editors](#validation-in-custom-editors)
- [Custom Editor Templates](#custom-editor-templates)

## Custom Editor Components

### Create Reusable Custom Editor

```typescript
import { Component, ViewChild, ElementRef } from '@angular/core';

// Custom Editor Component
@Component({
  selector: 'app-employee-editor',
  template: `
    <div class='custom-editor'>
      <input 
        #editorInput
        type='text'
        [(ngModel)]='value'
        placeholder='Select employee...'
        (keyup)='onSearch($event)'
        class='editor-input'>
      <ul *ngIf='filteredEmployees.length' class='dropdown-list'>
        <li *ngFor='let emp of filteredEmployees'
          (click)='selectEmployee(emp)'>
          <img [src]='emp.avatar' alt='Avatar' class='avatar'>
          <span>{{ emp.name }}</span>
        </li>
      </ul>
    </div>
  `,
  styles: [`
    .custom-editor {
      position: relative;
      width: 100%;
    }

    .editor-input {
      width: 100%;
      padding: 8px;
      border: 1px solid #ccc;
      border-radius: 4px;
      font-size: 14px;
    }

    .dropdown-list {
      position: absolute;
      top: 100%;
      left: 0;
      right: 0;
      background: white;
      border: 1px solid #ccc;
      border-top: none;
      list-style: none;
      padding: 0;
      margin: 0;
      z-index: 10;
      max-height: 200px;
      overflow-y: auto;
    }

    li {
      padding: 8px;
      display: flex;
      align-items: center;
      gap: 8px;
      cursor: pointer;
    }

    li:hover {
      background-color: #f5f5f5;
    }

    .avatar {
      width: 24px;
      height: 24px;
      border-radius: 50%;
    }
  `]
})
export class EmployeeEditorComponent {
  @ViewChild('editorInput') editorInput!: ElementRef;

  value: string = '';
  filteredEmployees: any[] = [];
  allEmployees = [
    { id: 1, name: 'Alice Johnson', avatar: 'assets/alice.jpg' },
    { id: 2, name: 'Bob Smith', avatar: 'assets/bob.jpg' },
    { id: 3, name: 'Carol Williams', avatar: 'assets/carol.jpg' }
  ];

  onSearch(event: any) {
    const searchText = event.target.value.toLowerCase();
    this.filteredEmployees = this.allEmployees.filter(emp =>
      emp.name.toLowerCase().includes(searchText)
    );
  }

  selectEmployee(emp: any) {
    this.value = emp.name;
    this.filteredEmployees = [];
  }

  onActionFailure(args: any) {
    console.error('Editor action failed:', args);
  }
}
```

### Use Custom Editor in TreeGrid

```typescript
@Component({
  selector: 'app-treegrid',
  template: `
    <ejs-treegrid 
      [dataSource]='data'
      [childMapping]='childMapping'
      [editSettings]='editSettings'>
      <e-columns>
        <e-column 
          field='TaskID' 
          headerText='Task ID' 
          width='90' 
          isPrimaryKey='true'>
        </e-column>
        <e-column 
          field='TaskName' 
          headerText='Task Name' 
          width='200'>
        </e-column>
        <e-column 
          field='AssignedTo' 
          headerText='Assigned To'
          editType='default'
          [editParams]='assignedToEditor'
          width='150'>
        </e-column>
      </e-columns>
    </ejs-treegrid>
  `,
  providers: [EditService]
})
export class AppComponent {
  public data: Object[] = [];
  public childMapping: string = 'subtasks';

  public editSettings = {
    mode: 'Dialog',
    allowEditing: true
  };

  // Pass custom editor component as edit parameter
  public assignedToEditor = {
    create: () => new EmployeeEditorComponent()
  };
}
```

---

## Editor Lifecycle

### Hook Into Editor Events

```typescript
@Component({
  selector: 'app-treegrid',
  template: `
    <ejs-treegrid 
      (actionBegin)='onActionBegin($event)'
      (actionComplete)='onActionComplete($event)'
      (beforeEdit)='onBeforeEdit($event)'
      (editCell)='onEditCell($event)'>
      <!-- columns -->
    </ejs-treegrid>
  `
})
export class AppComponent {
  // Called BEFORE edit mode is opened
  onBeforeEdit(args: any) {
    console.log('Edit starting for:', args.rowData.TaskName);
    
    // Initialize editor with current value
    args.initialValue = args.rowData[args.column.field];
    
    // Can prevent edit if conditions not met
    if (args.rowData.Status === 'Archived') {
      args.cancel = true;
      alert('Cannot edit archived task');
    }
  }

  // Called AFTER editor is rendered
  onEditCell(args: any) {
    console.log('Editor rendered for:', args.column.field);
    
    // Focus the editor input
    const editor = args.element as HTMLInputElement;
    if (editor) {
      setTimeout(() => editor.focus(), 100);
    }
  }

  // Called when action completes (save/cancel)
  onActionComplete(args: any) {
    if (args.requestType === 'save') {
      console.log('Saved:', args.data);
    } else if (args.requestType === 'cancel') {
      console.log('Cancelled edit');
    }
  }
}
```

---

## Validation in Custom Editors

### Validate Custom Editor

```typescript
@Component({
  selector: 'app-priority-editor',
  template: `
    <div class='priority-editor'>
      <select [(ngModel)]='value' (change)='onPriorityChange()'>
        <option value=''>Select Priority</option>
        <option value='Low'>Low</option>
        <option value='Medium'>Medium</option>
        <option value='High'>High</option>
      </select>
      <div *ngIf='error' class='error-message'>
        {{ error }}
      </div>
    </div>
  `,
  styles: [`
    .priority-editor {
      width: 100%;
    }

    select {
      width: 100%;
      padding: 8px;
      border: 1px solid #ccc;
      border-radius: 4px;
    }

    select.invalid {
      border-color: #f44336;
      background-color: #ffebee;
    }

    .error-message {
      color: #f44336;
      font-size: 12px;
      margin-top: 4px;
    }
  `]
})
export class PriorityEditorComponent {
  value: string = '';
  error: string = '';

  onPriorityChange() {
    // Validate
    if (!this.value) {
      this.error = 'Priority is required';
      return;
    }

    if (['Low', 'Medium', 'High'].indexOf(this.value) === -1) {
      this.error = 'Invalid priority value';
      return;
    }

    this.error = '';
  }

  validate(): boolean {
    return !this.error && this.value.length > 0;
  }
}
```

### Server-Side Validation

```typescript
onActionBegin(args: any) {
  if (args.requestType === 'save') {
    // Validate before sending to server
    if (!this.validateRecord(args.data)) {
      args.cancel = true;
      alert('Validation failed');
      return;
    }

    // Send to server for additional validation
    this.saveToServer(args.data).subscribe({
      next: (response) => {
        console.log('Saved successfully');
      },
      error: (error) => {
        args.cancel = true;
        alert('Server validation failed: ' + error.message);
      }
    });
  }
}

validateRecord(data: any): boolean {
  if (!data.TaskName || data.TaskName.trim() === '') {
    alert('Task name is required');
    return false;
  }

  if (data.Duration < 1 || data.Duration > 365) {
    alert('Duration must be between 1 and 365 days');
    return false;
  }

  return true;
}

saveToServer(data: any): Observable<any> {
  return this.http.post('/api/tasks/validate-and-save', data);
}
```

---

## Custom Editor Templates

### Editor Template in Column

```typescript
template: `
  <ejs-treegrid 
    [editSettings]='editSettings'>
    <e-columns>
      <!-- Text editor template -->
      <e-column field='TaskName' headerText='Task Name' width='200'>
        <ng-template #editTemplate let-data>
          <input 
            [(ngModel)]='data.TaskName'
            (blur)='onTaskNameBlur($event)'
            (keyup.enter)='saveEdit()'
            placeholder='Enter task name'
            class='editor-input'>
        </ng-template>
      </e-column>

      <!-- Dropdown editor template -->
      <e-column field='Status' headerText='Status' width='120'>
        <ng-template #editTemplate let-data>
          <select [(ngModel)]='data.Status' class='editor-select'>
            <option value=''>Select Status</option>
            <option value='NotStarted'>Not Started</option>
            <option value='InProgress'>In Progress</option>
            <option value='Completed'>Completed</option>
          </select>
        </ng-template>
      </e-column>

      <!-- Date picker editor template -->
      <e-column field='StartDate' headerText='Start Date' width='130' type='date'>
        <ng-template #editTemplate let-data>
          <input 
            type='date'
            [(ngModel)]='data.StartDate'
            class='editor-input'>
        </ng-template>
      </e-column>

      <!-- Numeric spinner editor template -->
      <e-column field='Duration' headerText='Duration' width='100'>
        <ng-template #editTemplate let-data>
          <ejs-numerictextbox 
            [(value)]='data.Duration'
            [min]='1'
            [max]='365'
            [step]='1'>
          </ejs-numerictextbox>
        </ng-template>
      </e-column>
    </e-columns>
  </ejs-treegrid>
`

onTaskNameBlur(event: any) {
  const value = event.target.value?.trim();
  if (!value) {
    console.warn('Task name cannot be empty');
  }
}

saveEdit() {
  this.treegrid.endEdit();
}
```

---

## Best Practices

1. **Keep Editors Simple** - Complex editors slow down editing UX
2. **Validate on Change** - Show errors in real-time where possible
3. **Focus Management** - Auto-focus input when editor opens
4. **Keyboard Support** - Support Enter to save, Escape to cancel
5. **Error Display** - Show clear validation error messages
6. **Async Validation** - Load dropdown data on-demand
7. **Accessibility** - Include labels and ARIA attributes
8. **Performance** - Avoid re-rendering editor on every keystroke

---

## Common Custom Editor Patterns

### Pattern 1: Autocomplete Dropdown
- Load options from API
- Filter as user types
- Show custom display (avatar + name)

### Pattern 2: Date Range Picker
- Select start and end dates
- Validate end date > start date
- Handle null dates

### Pattern 3: Color Picker
- Visual color selector
- Hex/RGB value support
- Validation for valid colors

### Pattern 4: Multi-Select
- Select multiple values from list
- Configurable selection limit
- Display selected as tags

### Pattern 5: Rich Text Editor
- Formatted text content
- Link/image insertion
- Toolbar with formatting options
