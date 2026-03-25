---
name: API Reference
description: 'API Reference for Syncfusion Angular TreeGrid - methods, properties, events, and interfaces documentation.'
---

# API Reference

Complete API reference for Syncfusion Angular TreeGrid component.

## Table of Contents
- [Property Naming Rules](#property-naming-rules)
- [Event Handling Rules](#event-handling-rules)
- [Component Properties](#component-properties)
- [Methods](#methods)
- [Events](#events)
- [Column Interface](#column-interface)
- [Edit Settings](#edit-settings)
- [Common Data Structures](#common-data-structures)

## Property Naming Rules

### Rule 1: Property Names are Case-Sensitive
**Severity**: 🔴 CRITICAL - Case mismatch causes silent failures

**Correct Property Names**:
```typescript
// ✅ CORRECT - Exact casing (camelCase for properties)
[allowPaging]='true'
[allowSorting]='true'
[allowFiltering]='true'
[allowRowDragAndDrop]='true'  // Capital 'D' and 'A'
[enableVirtualization]='true'
[enableInfiniteScrolling]='true'
[enablePersistence]='true'
[enableAdaptiveUI]='true'      // Capital 'A' and 'U'
[showColumnMenu]='true'
[childMapping]='subtasks'
[idMapping]='TaskID'
[parentIdMapping]='ParentID'
[hasChildMapping]='IsParent'

// ❌ WRONG - Incorrect casing (won't work)
[allowpaging]='true'           // lowercase 'p'
[allowsorting]='true'
[allowfiltering]='true'
[allowRowDragandDrop]='true'   // lowercase 'a' in 'and' - WRONG!
[enablevirtualization]='true'
[childmapping]='subtasks'      // lowercase 'c' and 'm'
```

**Column Property Casing**:
```typescript
// ✅ CORRECT
isPrimaryKey='true'
isFrozen='true'
isIdentity='true'
allowSorting='true'
allowFiltering='true'
allowSearching='true'
enableGrouping='true'
displayAsCheckBox='true'
allowReordering='true'
allowResizing='true'
showColumnMenu='true'

// ❌ WRONG
isprimarkey='true'
isfrozen='true'
allowsorting='true'
displayascheckbox='true'
```

---

### Rule 2: Event Names Follow Specific Pattern
**Severity**: 🟠 IMPORTANT - Event names must match exactly

**Correct Event Names**:
```typescript
// ✅ CORRECT - Exact event names
(rowSelecting)='onRowSelecting($event)'
(rowSelected)='onRowSelected($event)'
(rowDeselecting)='onRowDeselecting($event)'
(rowDeselected)='onRowDeselected($event)'

(rowDragStart)='onRowDragStart($event)'
(rowDrag)='onRowDrag($event)'
(rowDrop)='onRowDrop($event)'
(rowDragStop)='onRowDragStop($event)'

(dataBound)='onDataBound($event)'
(dataSourceChanged)='onDataSourceChanged($event)'
(dataStateChange)='onDataStateChange($event)'

(actionBegin)='onActionBegin($event)'
(actionComplete)='onActionComplete($event)'
(actionFailure)='onActionFailure($event)'

(queryCellInfo)='onQueryCellInfo($event)'
(rowDataBound)='onRowDataBound($event)'

(recordDoubleClick)='onRecordDoubleClick($event)'
(cellSave)='onCellSave($event)'

// ❌ WRONG - Incorrect event names
(rowdragstart)='...'           // lowercase 'd'
(dragStart)='...'              // Should be `rowDragStart`
(onRowDragStart)='...'         // Don't prefix with 'on'
(databound)='...'              // Should be `dataBound`
```

---

## Event Handling Rules

### Rule 3: Events with 'event.endEdit()' Require Proper Timing
**Severity**: 🟠 IMPORTANT - Async operations need event.endEdit()

**Requirement**:
```typescript
// ✅ CORRECT - Call endEdit() after async operations
(dataSourceChanged)='onDataSourceChanged($event)'

onDataSourceChanged(event: any): void {
  if (event.action === 'add') {
    // Make API call
    this.http.post('/api/tasks', event.data).subscribe({
      next: (response) => {
        // Update record with server response
        event.data.TaskID = response.TaskID;
        event.endEdit();  // ✅ Tell grid editing is done
      },
      error: () => {
        event.endEdit();  // ✅ Also call on error
      }
    });
  } else {
    event.endEdit();  // Call immediately for sync operations
  }
}

// ❌ WRONG - Not calling endEdit()
onDataSourceChanged(event: any): void {
  this.http.post('/api/tasks', event.data).subscribe({
    next: (response) => {
      // Missing event.endEdit() - Event listener stays stuck
    }
  });
}
```

---

### Rule 4: Prevent Default Behavior When Needed
**Severity**: 🟠 IMPORTANT - May need to stop default actions

**Requirement**:
```typescript
// ✅ CORRECT - Cancel event when validation fails
(rowSelecting)='onRowSelecting($event)'

onRowSelecting(event: any): void {
  if (isInvalid(event.rowData)) {
    event.cancel = true;  // ✅ Prevent selection
  }
}

// ✅ CORRECT - Cancel default delete behavior
(beginDelete)='onBeginDelete($event)'

onBeginDelete(event: any): void {
  // Show confirmation before delete
  if (!confirm('Delete this record?')) {
    event.cancel = true;  // ✅ Prevent deletion
  }
}

// ❌ WRONG - Allowing default when should cancel
onRowSelecting(event: any): void {
  if (isInvalid(event.rowData)) {
    // Missing event.cancel = true
  }
}
```

---

## Component Properties

### Basic Properties

```typescript
// DataSource
[dataSource]: Object[] | DataManager;

// Display
[rowHeight]: number; // Default: 36
[height]: string | number;
[width]: string | number;

// Features
[allowSelection]: boolean; // Default: true
[allowSorting]: boolean; // Default: false
[allowFiltering]: boolean; // Default: false
[allowPaging]: boolean; // Default: false
[allowRowDragAndDrop]: boolean; // Default: false
[allowExcelExport]: boolean; // Default: false
[allowPdfExport]: boolean; // Default: false
[enableVirtualization]: boolean; // Default: false
[enableColumnVirtualization]: boolean; // Default: false

// Child Records
[childMapping]: string; // Property name for child records
[parentValueMapping]: string; // Foreign key mapping
[expandStateMapping]: string; // Property name for expand state

// Primary Key
[idMapping]: string; // Unique identifier field
```

### Selection and Behavior

```typescript
[selectionSettings]: SelectionSettings;
// {
//   type: 'Single' | 'Multiple' | 'Checkbox'
//   mode: 'Row' | 'Cell' | 'Both'
//   cellSelectionMode: 'Flow' | 'Box' | 'BoxMultiSelect'
// }

[gridLines]: 'Both' | 'Horizontal' | 'Vertical' | 'None';
[rowTemplate]: string | Function; // Custom row template
[detailTemplate]: string | Function; // Expand/Detail template
[toolbarItems]: Object[] | string[]; // Toolbar items
[contextMenuItems]: Object[] | string[]; // Context menu items
```

## Methods

### Data Operations

```typescript
// Get current data
getData(): Object[];

// Refresh grid
refresh(): void;
refreshRows(indexes: number[], isNow?: boolean): void;

// Add record
addRecord(data: Object, index?: number, position?: 'Top' | 'Bottom'): void;

// Update record
setRowData(index: number, data: Object): void;

// Delete record
deleteRecord(fieldname?: string, value?: Object | Object[]): void;
deleteRow(index: number): void;

// Clear records
clear(): void;
```

### Selection

```typescript
// Row selection
selectRow(index: number | number[], isToggle?: boolean): void;
selectRows(indexesOrElements: number[] | HTMLTableRowElement[]): void;
getSelectedRowIndexes(): number[];
getSelectedRecords(): Object[];

// Cell selection
selectCell(cellIndex: IPosition, isToggle?: boolean): void;
getSelectedRowCellIndexes(): IPosition[];
```

### Expand/Collapse

```typescript
// Expand
expandRow(row: HTMLTableRowElement | number): Promise<Object>;
expandAtLevel(level: number): Promise<Object>;
expandAll(): Promise<Object>;

// Collapse
collapseRow(row: HTMLTableRowElement | number): Promise<Object>;
collapseAtLevel(level: number): Promise<Object>;
collapseAll(): Promise<Object>;

// Toggle
toggleRow(row: HTMLTableRowElement | number): Promise<Object>;
```

### Filtering & Sorting

```typescript
// Filter
filterByColumn(fieldName: string, filterOperator: string, filterValue: string | number | Date, predicateOperator?: string, matchCase?: boolean): void;
clearFiltering(): void;

// Sort
sortByColumn(fieldName: string, direction: 'Ascending' | 'Descending', isMultiSort?: boolean): void;
clearSorting(): void;

// Search
search(searchString: string): void;
```

### Export

```typescript
// Excel Export
excelExport(excelExportProperties?: ExcelExportProperties): Promise<Object>;

// PDF Export
pdfExport(pdfExportProperties?: PdfExportProperties): Promise<Object>;

// CSV Export
csvExport(csvExportProperties?: CsvExportProperties): Promise<Object>;
```

### Editing

```typescript
// Edit row
startEdit(target?: Object): void;
endEdit(): void;
cancelEdit(): void;

// Edit cell
startCellEdit(target?: Object): void;
endCellEdit(): void;

// Batch edit
batchSave(): void;
batchCancel(): void;

// Get edit parameters
getEditSettings(): EditSettingsModel;
```

### UI State

```typescript
// Columns
getVisibleColumns(): ColumnModel[];
getColumns(): ColumnModel[];
hideColumns(keys: string | string[]): void;
showColumns(keys: string | string[]): void;

// Get elements
getRowByIndex(index: number): Element;
getRowByKey(key: string): Element;
getCellByIndex(rowIndex: number, colIndex: number): Element;

// Print
print(): void;
```

### Pagination

```typescript
[pageSettings]: PageSettingsModel;
// {
//   pageSize: number;
//   pageCount: number;
//   currentPage: number;
//   totalRecordsCount: number;
// }

goToPage(pageNumber: number): void;
```

## Events

### Data Events

```typescript
// When datasource changes
(dataSourceChanged): EventEmitter<DataSourceChangedEventArgs>;

// After grid rendered
(dataBound): EventEmitter<Object>;

// On action failure
(actionFailure): EventEmitter<FailureEventArgs>;

// On record change
(recordDoubleClick): EventEmitter<RecordDoubleClickEventArgs>;
```

### Selection Events

```typescript
// Row selected
(rowSelecting): EventEmitter<RowSelectEventArgs>;
(rowSelected): EventEmitter<RowSelectEventArgs>;

// Row deselected
(rowDeselecting): EventEmitter<RowDeselectEventArgs>;
(rowDeselected): EventEmitter<RowDeselectEventArgs>;

// Cell selected
(cellSelecting): EventEmitter<CellSelectingEventArgs>;
(cellSelected): EventEmitter<CellSelectEventArgs>;
```

### Expand/Collapse Events

```typescript
// Row expanding
(rowExpanding): EventEmitter<RowExpandingEventArgs>;

// Row expanded
(rowExpanded): EventEmitter<RowExpandedEventArgs>;

// Row collapsing
(rowCollapsing): EventEmitter<RowCollapsingEventArgs>;

// Row collapsed
(rowCollapsed): EventEmitter<RowCollapsedEventArgs>;
```

### Edit Events

```typescript
// Edit start
(beginEdit): EventEmitter<BeginEditEventArgs>;

// Edit cell start
(cellEdit): EventEmitter<CellEditEventArgs>;

// Cell save
(cellSave): EventEmitter<CellSaveEventArgs>;

// Batch save
(beforeBatchSave): EventEmitter<BeforeBatchSaveEventArgs>;
(batchSave): EventEmitter<BatchSaveEventArgs>;

// Before delete
(beforeDelete): EventEmitter<BeforeDeleteEventArgs>;
```

### User Interaction Events

```typescript
// Drag and drop
(rowDragStart): EventEmitter<RowDragEventArgs>;
(rowDrag): EventEmitter<RowDragEventArgs>;
(rowDrop): EventEmitter<RowDropEventArgs>;

// Right click menu
(contextMenuClick): EventEmitter<ContextMenuClickEventArgs>;
(contextMenuOpen): EventEmitter<ContextMenuOpenEventArgs>;

// Toolbar click
(toolbarClick): EventEmitter<ToolbarClickEventArgs>;

// Column resize
(columnResizeStart): EventEmitter<ColumnResizeEventArgs>;
(columnResizing): EventEmitter<ColumnResizeEventArgs>;
(columnResizeStop): EventEmitter<ColumnResizeEventArgs>;

// Column reorder
(columnDragStart): EventEmitter<ColumnDragEventArgs>;
(columnDrop): EventEmitter<ColumnDropEventArgs>;
```

### Filter & Sort Events

```typescript
// Filter
(filteringEventHandler): EventEmitter<FilteringEventArgs>;

// Sort
(sortingEventHandler): EventEmitter<SortingEventArgs>;
```

## Column Interface

```typescript
interface ColumnModel {
  field: string; // Data field name
  headerText: string; // Column header text
  width?: number | string; // Column width
  type?: 'text' | 'number' | 'date' | 'boolean'; // Data type
  isPrimaryKey?: boolean; // Is primary key
  
  // Display
  visible?: boolean; // Show/hide
  textAlign?: 'Left' | 'Right' | 'Center' | 'Justify';
  headerTextAlign?: 'Left' | 'Right' | 'Center';
  displayAsCheckBox?: boolean; // Show as checkbox
  
  // Formatting
  format?: string; // Format string
  template?: string | Function; // Cell template
  headerTemplate?: string | Function; // Header template
  footerTemplate?: string | Function; // Footer template
  
  // Validation
  validationRules?: Object; // Validation rules
  
  // Filtering
  filter?: FilterSettingsModel;
  allowFiltering?: boolean;
  
  // Sorting
  allowSorting?: boolean;
  sortComparer?: (a: Object, b: Object) => number;
  
  // Editing
  editType?: 'Default' | 'Dropdown' | 'Numeric' | 'NumericTextBox' | 'TextArea';
  edit?: Object; // Edit parameters
  allowEditing?: boolean;
  
  // Other
  minWidth?: number;
  showColumnMenu?: boolean;
  enableGroupByFormat?: boolean;
}
```

## Edit Settings

```typescript
interface EditSettingsModel {
  // Edit mode
  mode?: 'Normal' | 'Dialog' | 'Batch' | 'Cell';
  
  // Permissions
  allowEditing?: boolean;
  allowAdding?: boolean;
  allowDeleting?: boolean;
  allowEditOnDblClick?: boolean; // Edit on double click
  
  // Dialog
  dialogEditorTemplateID?: string;
  template?: (props: IEditCell) => any;
}
```

## Common Data Structures

### Selection Settings

```typescript
interface SelectionSettings {
  type?: 'Single' | 'Multiple' | 'Checkbox';
  mode?: 'Row' | 'Cell' | 'Both';
  cellSelectionMode?: 'Flow' | 'Box' | 'BoxMultiSelect';
  checkboxOnly?: boolean; // Select only via checkbox
}
```

### Filter Settings

```typescript
interface FilterSettingsModel {
  type?: 'Excel' | 'FilterBar'; // Filter bar type
  mode?: 'Immediate' | 'OnEnter'; // Apply filter on Enter key
  columns?: PredicateModel[]; // Initial filters
  hierarchyMode?: 'Parent' | 'Child' | 'Both';
  ignoreAccent?: boolean;
}

interface PredicateModel {
  field: string;
  operator: string; // 'equal', 'notequal', 'greaterthan', etc.
  value: string | number | Date | boolean;
  predicateOperator?: 'and' | 'or';
  matchCase?: boolean;
}
```

### Sort Settings

```typescript
interface SortSettingsModel {
  columns?: SortDescriptorModel[];
  ignoreAccent?: boolean;
}

interface SortDescriptorModel {
  field: string;
  direction: 'Ascending' | 'Descending';
  isFromGroup?: boolean;
}
```

### Page Settings

```typescript
interface PageSettingsModel {
  currentPage?: number;
  pageSize?: number;
  pageCount?: number;
  totalRecordsCount?: number;
}
```

### Edit Row Arguments

```typescript
interface BeginEditEventArgs {
  data: Object; // Row data
  rowData: Object;
  rowIndex: number;
  cancel: boolean; // Cancel edit
  requestType: 'save' | 'beginedit' | 'add';
  type: string;
}

interface CellEditEventArgs {
  data: Object; // Cell data
  previousValue: any; // Original value
  value: any; // New value
  column: ColumnModel;
  rowData: Object;
  rowIndex: number;
  cancel: boolean;
}

interface CellSaveEventArgs {
  data: Object;
  previousValue: any;
  value: any;
  column: ColumnModel;
  rowIndex: number;
  cancel: boolean;
}
```

### Row Drop Arguments

```typescript
interface RowDropEventArgs {
  data: Object[]; // Dragged data
  rowIndex: number;
  dropIndex: number;
  dropPosition: 'above' | 'below' | 'child';
  cancel: boolean;
  target: Element;
}
```

## Usage Examples

### Complete Component Setup

```typescript
import { Component, ViewChild } from '@angular/core';
import { TreeGridComponent } from '@syncfusion/ej2-angular-treegrid';

@Component({
  selector: 'app-treegrid',
  template: `
    <ejs-treegrid 
      #treegrid
      [dataSource]='data'
      [allowSelection]='true'
      [allowSorting]='true'
      [allowFiltering]='true'
      [allowPaging]='true'
      [editSettings]='editSettings'
      [pageSettings]='pageSettings'
      [selectionSettings]='selectionSettings'
      (actionFailure)='onActionFailure($event)'
      (dataBound)='onDataBound($event)'>
      <e-columns>
        <e-column field='TaskID' headerText='ID' width='60'></e-column>
        <e-column field='TaskName' headerText='Task Name' width='200'></e-column>
        <e-column field='Progress' headerText='Progress' width='100'></e-column>
      </e-columns>
    </ejs-treegrid>
  `
})
export class AppComponent {
  @ViewChild('treegrid') treegrid!: TreeGridComponent;
  
  public data: Object[] = [];

  public editSettings = {
    allowEditing: true,
    allowAdding: true,
    allowDeleting: true,
    mode: 'Dialog'
  };

  public pageSettings = {
    pageSize: 20,
    pageCount: 5
  };

  public selectionSettings = {
    type: 'Multiple',
    mode: 'Row'
  };

  onActionFailure(e: any) {
    console.error('Error:', e);
  }

  onDataBound() {
    console.log('Grid data binding complete');
  }

  // API method usage
  addNewTask() {
    this.treegrid.addRecord({ TaskID: 999, TaskName: 'New Task' });
  }

  deleteSelectedRows() {
    const selected = this.treegrid.getSelectedRowIndexes();
    selected.forEach(index => this.treegrid.deleteRow(index));
  }

  exportData() {
    this.treegrid.excelExport();
  }
}
```
