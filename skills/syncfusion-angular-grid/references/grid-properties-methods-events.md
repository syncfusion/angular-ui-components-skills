# Grid Component - Properties, Methods, and Events Reference

## Table of Contents
- [Properties Overview](#properties-overview)
- [Core Data Properties](#core-data-properties)
- [Layout & Display Properties](#layout--display-properties)
- [Feature Enablement Properties](#feature-enablement-properties)
- [Selection & Editing Properties](#selection--editing-properties)
- [Methods Overview](#methods-overview)
- [Data Manipulation Methods](#data-manipulation-methods)
- [Selection Methods](#selection-methods)
- [Column Management Methods](#column-management-methods)
- [Navigation & Paging Methods](#navigation--paging-methods)
- [Events Overview](#events-overview)
- [Data Events](#data-events)
- [Interaction Events](#interaction-events)
- [Practical Examples](#practical-examples)

---

## Properties Overview

The Grid component has 95+ configurable properties. Here are the most commonly used ones organized by category.

### Quick Reference Table

| Category | Key Properties |
|----------|----------------|
| **Data Binding** | `dataSource`, `columns`, `query` |
| **Paging** | `allowPaging`, `pageSettings` |
| **Sorting** | `allowSorting`, `allowMultiSorting`, `sortSettings` |
| **Filtering** | `allowFiltering`, `filterSettings` |
| **Selection** | `allowSelection`, `selectionSettings` |
| **Editing** | `editSettings` |
| **Layout** | `height`, `width`, `gridLines`, `rowHeight` |
| **Export** | `allowExcelExport`, `allowPdfExport` |
| **Grouping** | `allowGrouping`, `groupSettings` |

---

## Core Data Properties

### dataSource
**Type:** `Object | DataManager | DataResult`  
**Default:** `[]`  
**Description:** Primary data source for rendering grid rows

```typescript
import { Component } from '@angular/core';
import { GridModule } from '@syncfusion/ej2-angular-grids';

@Component({
  selector: 'app-grid-datasource',
  standalone: true,
  imports: [GridModule],
  template: `
    <ejs-grid [dataSource]="data">
      <e-columns>
        <e-column field="OrderID" headerText="Order ID" width="100"></e-column>
        <e-column field="CustomerID" headerText="Customer" width="100"></e-column>
        <e-column field="Freight" headerText="Freight" width="100" format="C2"></e-column>
      </e-columns>
    </ejs-grid>
  `
})
export class GridDataSourceComponent {
  data = [
    { OrderID: 10001, CustomerID: 'ALFKI', Freight: 32.38 },
    { OrderID: 10002, CustomerID: 'ANATR', Freight: 11.61 }
  ];
}
```

### columns
**Type:** `Column[] | string[] | ColumnModel[]`  
**Default:** `[]`  
**Description:** Schema definition for grid columns. Auto-generated from dataSource if empty

```typescript
@Component({
  selector: 'app-columns-config',
  standalone: true,
  imports: [GridModule],
  template: `
    <ejs-grid [dataSource]="data">
      <e-columns>
        <e-column field="OrderID" headerText="Order ID" width="100"></e-column>
        <e-column field="CustomerID" headerText="Customer Name" width="150"></e-column>
        <e-column field="ShipCity" headerText="Ship City" width="150"></e-column>
      </e-columns>
    </ejs-grid>
  `
})
export class ColumnsConfigComponent {
  data: any[] = [];
}
```

### query
**Type:** `Query`  
**Default:** `null`  
**Description:** External Query to be executed along with data processing

```typescript
import { Component, OnInit } from '@angular/core';
import { GridModule } from '@syncfusion/ej2-angular-grids';
import { Query, DataManager, UrlAdaptor } from '@syncfusion/ej2-data';

@Component({
  selector: 'app-grid-query',
  standalone: true,
  imports: [GridModule],
  template: `
    <ejs-grid [dataSource]="gridData" [query]="query">
      <e-columns>
        <e-column field="OrderID" headerText="Order ID" width="100"></e-column>
        <e-column field="CustomerID" headerText="Customer" width="100"></e-column>
        <e-column field="Freight" headerText="Freight" width="100"></e-column>
      </e-columns>
    </ejs-grid>
  `
})
export class GridQueryComponent implements OnInit {
  gridData!: DataManager;
  query!: Query;

  ngOnInit(): void {
    this.gridData = new DataManager({ url: 'api/orders', adaptor: new UrlAdaptor() });
    this.query = new Query()
      .select(['OrderID', 'CustomerID', 'Freight'])
      .where('Freight', 'greaterThan', 100);
  }
}
```

---

## Layout & Display Properties

### height
**Type:** `string | number`  
**Default:** `'auto'`  
**Description:** Scrollable height of grid content

```typescript
@Component({
  selector: 'app-grid-height',
  standalone: true,
  imports: [GridModule],
  template: `
    <ejs-grid [dataSource]="data" [height]="400">
      <e-columns>
        <e-column field="OrderID" headerText="Order ID" width="100"></e-column>
      </e-columns>
    </ejs-grid>

    <!-- Or with string value -->
    <ejs-grid [dataSource]="data" height="500px">
      <e-columns>
        <e-column field="OrderID" headerText="Order ID" width="100"></e-column>
      </e-columns>
    </ejs-grid>
  `
})
export class GridHeightComponent {
  data: any[] = [];
}
```

### width
**Type:** `string | number`  
**Default:** `'auto'`  
**Description:** Grid width

```typescript
@Component({
  selector: 'app-grid-width',
  standalone: true,
  imports: [GridModule],
  template: `
    <ejs-grid [dataSource]="data" width="100%">
      <e-columns>
        <e-column field="OrderID" headerText="Order ID" width="100"></e-column>
      </e-columns>
    </ejs-grid>
  `
})
export class GridWidthComponent {
  data: any[] = [];
}
```

### rowHeight
**Type:** `number`  
**Default:** `null` (auto-calculated)  
**Description:** Height for all grid rows

```typescript
@Component({
  selector: 'app-grid-rowheight',
  standalone: true,
  imports: [GridModule],
  template: `
    <ejs-grid [dataSource]="data" [rowHeight]="40">
      <e-columns>
        <e-column field="OrderID" headerText="Order ID" width="100"></e-column>
      </e-columns>
    </ejs-grid>
  `
})
export class GridRowHeightComponent {
  data: any[] = [];
}
```

### gridLines
**Type:** `GridLine` (Both | None | Horizontal | Vertical | Default)  
**Default:** `Default`  
**Description:** Grid line display mode

```typescript
@Component({
  selector: 'app-grid-lines',
  standalone: true,
  imports: [GridModule],
  template: `
    <ejs-grid [dataSource]="data" gridLines="Both">
      <e-columns>
        <e-column field="OrderID" headerText="Order ID" width="100"></e-column>
      </e-columns>
    </ejs-grid>
  `
})
export class GridLinesComponent {
  data: any[] = [];
}
```

### allowTextWrap
**Type:** `boolean`  
**Default:** `false`  
**Description:** Enables text wrapping in cells

```typescript
@Component({
  selector: 'app-grid-textwrap',
  standalone: true,
  imports: [GridModule],
  template: `
    <ejs-grid [dataSource]="data" [allowTextWrap]="true">
      <e-columns>
        <e-column field="OrderID" headerText="Order ID" width="100"></e-column>
      </e-columns>
    </ejs-grid>
  `
})
export class GridTextWrapComponent {
  data: any[] = [];
}
```

### enableAltRow
**Type:** `boolean`  
**Default:** `true`  
**Description:** Renders alternating row styling

```typescript
@Component({
  selector: 'app-grid-altrow',
  standalone: true,
  imports: [GridModule],
  template: `
    <ejs-grid [dataSource]="data" [enableAltRow]="true">
      <e-columns>
        <e-column field="OrderID" headerText="Order ID" width="100"></e-column>
      </e-columns>
    </ejs-grid>
  `
})
export class GridAltRowComponent {
  data: any[] = [];
}
```

### enableHover
**Type:** `boolean`  
**Default:** `true`  
**Description:** Enables row hover effect

```typescript
@Component({
  selector: 'app-grid-hover',
  standalone: true,
  imports: [GridModule],
  template: `
    <ejs-grid [dataSource]="data" [enableHover]="true">
      <e-columns>
        <e-column field="OrderID" headerText="Order ID" width="100"></e-column>
      </e-columns>
    </ejs-grid>
  `
})
export class GridHoverComponent {
  data: any[] = [];
}
```

### enableStickyHeader
**Type:** `boolean`  
**Default:** `false`  
**Description:** Makes column headers visible while scrolling

```typescript
@Component({
  selector: 'app-grid-stickyheader',
  standalone: true,
  imports: [GridModule],
  template: `
    <ejs-grid [dataSource]="data" [enableStickyHeader]="true">
      <e-columns>
        <e-column field="OrderID" headerText="Order ID" width="100"></e-column>
      </e-columns>
    </ejs-grid>
  `
})
export class GridStickyHeaderComponent {
  data: any[] = [];
}
```

---

## Feature Enablement Properties

### allowPaging
**Type:** `boolean`  
**Default:** `false`  
**Description:** Enables pager rendering at grid footer for page navigation

```typescript
import { Component } from '@angular/core';
import { GridModule, PageService } from '@syncfusion/ej2-angular-grids';

@Component({
  selector: 'app-grid-paging',
  standalone: true,
  imports: [GridModule],
  providers: [PageService],
  template: `
    <ejs-grid [dataSource]="data" [allowPaging]="true">
      <e-columns>
        <e-column field="OrderID" headerText="Order ID" width="100"></e-column>
      </e-columns>
    </ejs-grid>
  `
})
export class GridPagingComponent {
  data: any[] = [];
}
```

### pageSettings
**Type:** `PageSettingsModel`  
**Default:** `{currentPage: 1, pageSize: 12, pageCount: 8, ...}`  
**Description:** Configures pager behavior

```typescript
@Component({
  selector: 'app-grid-pagesettings',
  standalone: true,
  imports: [GridModule],
  providers: [PageService],
  template: `
    <ejs-grid [dataSource]="data" [allowPaging]="true" [pageSettings]="pageSettings">
      <e-columns>
        <e-column field="OrderID" headerText="Order ID" width="100"></e-column>
      </e-columns>
    </ejs-grid>
  `
})
export class GridPageSettingsComponent {
  data: any[] = [];
  pageSettings = { pageSize: 20, currentPage: 1 };
}
```

### allowSorting
**Type:** `boolean`  
**Default:** `false`  
**Description:** Enables sorting when clicking column headers

```typescript
import { Component } from '@angular/core';
import { GridModule, SortService } from '@syncfusion/ej2-angular-grids';

@Component({
  selector: 'app-grid-sorting',
  standalone: true,
  imports: [GridModule],
  providers: [SortService],
  template: `
    <ejs-grid [dataSource]="data" [allowSorting]="true">
      <e-columns>
        <e-column field="OrderID" headerText="Order ID" width="100"></e-column>
      </e-columns>
    </ejs-grid>
  `
})
export class GridSortingComponent {
  data: any[] = [];
}
```

### allowMultiSorting
**Type:** `boolean`  
**Default:** `false`  
**Description:** Allows sorting multiple columns

```typescript
@Component({
  selector: 'app-grid-multisorting',
  standalone: true,
  imports: [GridModule],
  providers: [SortService],
  template: `
    <ejs-grid [dataSource]="data" [allowSorting]="true" [allowMultiSorting]="true">
      <e-columns>
        <e-column field="OrderID" headerText="Order ID" width="100"></e-column>
      </e-columns>
    </ejs-grid>
  `
})
export class GridMultiSortingComponent {
  data: any[] = [];
}
```

### allowFiltering
**Type:** `boolean`  
**Default:** `false`  
**Description:** Displays filter bar for record filtering

```typescript
import { Component } from '@angular/core';
import { GridModule, FilterService } from '@syncfusion/ej2-angular-grids';

@Component({
  selector: 'app-grid-filtering',
  standalone: true,
  imports: [GridModule],
  providers: [FilterService],
  template: `
    <ejs-grid [dataSource]="data" [allowFiltering]="true">
      <e-columns>
        <e-column field="OrderID" headerText="Order ID" width="100"></e-column>
      </e-columns>
    </ejs-grid>
  `
})
export class GridFilteringComponent {
  data: any[] = [];
}
```

### allowSelection
**Type:** `boolean`  
**Default:** `true`  
**Description:** Enables row/cell selection with highlighting

```typescript
@Component({
  selector: 'app-grid-selection',
  standalone: true,
  imports: [GridModule],
  template: `
    <ejs-grid [dataSource]="data" [allowSelection]="true">
      <e-columns>
        <e-column field="OrderID" headerText="Order ID" width="100"></e-column>
      </e-columns>
    </ejs-grid>
  `
})
export class GridSelectionComponent {
  data: any[] = [];
}
```

### allowGrouping
**Type:** `boolean`  
**Default:** `false`  
**Description:** Enables dynamic column grouping via drag-drop

```typescript
import { Component } from '@angular/core';
import { GridModule, GroupService } from '@syncfusion/ej2-angular-grids';

@Component({
  selector: 'app-grid-grouping',
  standalone: true,
  imports: [GridModule],
  providers: [GroupService],
  template: `
    <ejs-grid [dataSource]="data" [allowGrouping]="true">
      <e-columns>
        <e-column field="OrderID" headerText="Order ID" width="100"></e-column>
      </e-columns>
    </ejs-grid>
  `
})
export class GridGroupingComponent {
  data: any[] = [];
}
```

### allowExcelExport
**Type:** `boolean`  
**Default:** `false`  
**Description:** Enables Excel export functionality

```typescript
import { Component } from '@angular/core';
import { GridModule, ExcelExportService } from '@syncfusion/ej2-angular-grids';

@Component({
  selector: 'app-grid-excelexport',
  standalone: true,
  imports: [GridModule],
  providers: [ExcelExportService],
  template: `
    <ejs-grid [dataSource]="data" [allowExcelExport]="true">
      <e-columns>
        <e-column field="OrderID" headerText="Order ID" width="100"></e-column>
      </e-columns>
    </ejs-grid>
  `
})
export class GridExcelExportComponent {
  data: any[] = [];
}
```

### allowPdfExport
**Type:** `boolean`  
**Default:** `false`  
**Description:** Enables PDF export functionality

```typescript
import { Component } from '@angular/core';
import { GridModule, PdfExportService } from '@syncfusion/ej2-angular-grids';

@Component({
  selector: 'app-grid-pdfexport',
  standalone: true,
  imports: [GridModule],
  providers: [PdfExportService],
  template: `
    <ejs-grid [dataSource]="data" [allowPdfExport]="true">
      <e-columns>
        <e-column field="OrderID" headerText="Order ID" width="100"></e-column>
      </e-columns>
    </ejs-grid>
  `
})
export class GridPdfExportComponent {
  data: any[] = [];
}
```

---

## Selection & Editing Properties

### selectionSettings
**Type:** `SelectionSettingsModel`  
**Description:** Configures selection mode, cellSelectionMode, and type

```typescript
@Component({
  selector: 'app-grid-selection-settings',
  standalone: true,
  imports: [GridModule],
  template: `
    <ejs-grid [dataSource]="data" [selectionSettings]="selectionSettings">
      <e-columns>
        <e-column field="OrderID" headerText="Order ID" width="100"></e-column>
      </e-columns>
    </ejs-grid>
  `
})
export class GridSelectionSettingsComponent {
  data: any[] = [];
  selectionSettings = {
    mode: 'Row',           // Row, Cell, or Column
    type: 'Multiple',      // Single or Multiple
    cellSelectionMode: 'Flow' // Flow or Box
  };
}
```

### editSettings
**Type:** `EditSettingsModel`  
**Description:** Configures editing behavior, modes, and dialogs

```typescript
import { Component } from '@angular/core';
import { GridModule, EditService } from '@syncfusion/ej2-angular-grids';

@Component({
  selector: 'app-grid-editing',
  standalone: true,
  imports: [GridModule],
  providers: [EditService],
  template: `
    <ejs-grid [dataSource]="data" [editSettings]="editSettings">
      <e-columns>
        <e-column field="OrderID" headerText="Order ID" width="100"></e-column>
      </e-columns>
    </ejs-grid>
  `
})
export class GridEditingComponent {
  data: any[] = [];
  editSettings = {
    allowAdding: true,
    allowEditing: true,
    allowDeleting: true,
    mode: 'Dialog'  // Dialog, Batch, InlineAdd, InlineEdit, etc.
  };
}
```

---

## Methods Overview

The Grid component provides 137+ methods organized by functionality. Here are the most commonly used ones.

---

## Data Manipulation Methods

### addRecord()
**Parameters:** `data?: Object, index?: number`  
**Return:** `void`  
**Description:** Adds new record(s) to grid (requires `editSettings.allowEditing: true`)

```typescript
import { Component, ViewChild } from '@angular/core';
import { GridComponent, GridModule, EditService } from '@syncfusion/ej2-angular-grids';

@Component({
  selector: 'app-grid-addrecord',
  standalone: true,
  imports: [GridModule],
  providers: [EditService],
  template: `
    <div>
      <button (click)="handleAddRecord()">Add Record</button>
      
      <ejs-grid #grid [dataSource]="data" [editSettings]="editSettings">
        <e-columns>
          <e-column field="OrderID" headerText="Order ID" width="100"></e-column>
        </e-columns>
      </ejs-grid>
    </div>
  `
})
export class GridAddRecordComponent {
  @ViewChild('grid') gridComponent!: GridComponent;
  data: any[] = [];
  editSettings = { allowAdding: true, allowEditing: true };

  handleAddRecord(): void {
    const newRecord = {
      OrderID: 10003,
      CustomerID: 'BLONP',
      Freight: 25.00
    };
    this.gridComponent.addRecord(newRecord);
  }
}
```

### setCellValue()
**Parameters:** `key: string | number, field: string, value: string | number | boolean | Date | null`  
**Return:** `void`  
**Description:** Updates single cell value by primary key

```typescript
@Component({
  selector: 'app-grid-setcellvalue',
  standalone: true,
  imports: [GridModule],
  template: ``
})
export class GridSetCellValueComponent {
  @ViewChild('grid') gridComponent!: GridComponent;

  handleUpdateCell(): void {
    this.gridComponent.setCellValue(10001, 'Freight', 50.00);
  }
}
```

### setRowData()
**Parameters:** `key: string | number, rowData?: Object`  
**Return:** `void`  
**Description:** Updates entire row by primary key

```typescript
@Component({
  selector: 'app-grid-setrowdata',
  standalone: true,
  imports: [GridModule],
  template: ``
})
export class GridSetRowDataComponent {
  @ViewChild('grid') gridComponent!: GridComponent;

  handleUpdateRow(): void {
    const updatedData = {
      OrderID: 10001,
      CustomerID: 'UPDATED',
      Freight: 100.00
    };
    this.gridComponent.setRowData(10001, updatedData);
  }
}
```

### deleteRecord()
**Parameters:** `fieldname?: string, data?: Object`  
**Return:** `void`  
**Description:** Deletes record (requires `editSettings.allowDeleting: true`)

```typescript
@Component({
  selector: 'app-grid-deleterecord',
  standalone: true,
  imports: [GridModule],
  template: ``
})
export class GridDeleteRecordComponent {
  @ViewChild('grid') gridComponent!: GridComponent;

  handleDeleteRecord(): void {
    this.gridComponent.deleteRecord('OrderID', { OrderID: 10001 });
  }
}
```

### getBatchChanges()
**Return:** `Object`  
**Description:** Gets added, edited, deleted data before bulk save

```typescript
@Component({
  selector: 'app-grid-batchchanges',
  standalone: true,
  imports: [GridModule],
  template: ``
})
export class GridBatchChangesComponent {
  @ViewChild('grid') gridComponent!: GridComponent;

  getBatchUpdates(): void {
    const changes = this.gridComponent.getBatchChanges();
    console.log('Added:', changes.addedRecords);
    console.log('Modified:', changes.changedRecords);
    console.log('Deleted:', changes.deletedRecords);
  }
}
```

### changeDataSource()
**Parameters:** `dataSource?: Object | DataManager | DataResult, columns?: Column[]`  
**Return:** `void`  
**Description:** Changes datasource and columns completely

```typescript
@Component({
  selector: 'app-grid-changedatasource',
  standalone: true,
  imports: [GridModule],
  template: ``
})
export class GridChangeDataSourceComponent {
  @ViewChild('grid') gridComponent!: GridComponent;

  changeData(): void {
    const newData = [
      { OrderID: 20001, CustomerID: 'NEWCUST', Freight: 50 }
    ];
    this.gridComponent.changeDataSource(newData);
  }
}
```

---

## Selection Methods

### selectRow()
**Parameters:** `index: number, isToggle?: boolean`  
**Return:** `void`  
**Description:** Selects row by index

```typescript
@Component({
  selector: 'app-grid-selectrow',
  standalone: true,
  imports: [GridModule],
  template: ``
})
export class GridSelectRowComponent {
  @ViewChild('grid') gridComponent!: GridComponent;

  handleSelectRow(): void {
    this.gridComponent.selectRow(0);  // Select first row
  }
}
```

### selectRows()
**Parameters:** `rowIndexes: number[]`  
**Return:** `void`  
**Description:** Selects multiple rows by indexes

```typescript
@Component({
  selector: 'app-grid-selectrows',
  standalone: true,
  imports: [GridModule],
  template: ``
})
export class GridSelectRowsComponent {
  @ViewChild('grid') gridComponent!: GridComponent;

  handleSelectMultipleRows(): void {
    this.gridComponent.selectRows([0, 2, 4]);  // Select rows at index 0, 2, 4
  }
}
```

### getSelectedRowIndexes()
**Return:** `number[]`  
**Description:** Gets selected row indexes

```typescript
@Component({
  selector: 'app-grid-getselectedindexes',
  standalone: true,
  imports: [GridModule],
  template: ``
})
export class GridGetSelectedIndexesComponent {
  @ViewChild('grid') gridComponent!: GridComponent;

  getSelectedIndexes(): void {
    const indexes = this.gridComponent.getSelectedRowIndexes();
    console.log('Selected rows:', indexes);
  }
}
```

### getSelectedRecords()
**Return:** `Object[]`  
**Description:** Gets selected row data

```typescript
@Component({
  selector: 'app-grid-getselecteddata',
  standalone: true,
  imports: [GridModule],
  template: ``
})
export class GridGetSelectedDataComponent {
  @ViewChild('grid') gridComponent!: GridComponent;

  getSelectedData(): void {
    const selectedRecords = this.gridComponent.getSelectedRecords();
    console.log('Selected data:', selectedRecords);
  }
}
```

### clearRowSelection()
**Return:** `void`  
**Description:** Deselects all rows

```typescript
@Component({
  selector: 'app-grid-clearselection',
  standalone: true,
  imports: [GridModule],
  template: ``
})
export class GridClearSelectionComponent {
  @ViewChild('grid') gridComponent!: GridComponent;

  handleClearSelection(): void {
    this.gridComponent.clearRowSelection();
  }
}
```

### selectCell()
**Parameters:** `cellIndex: IIndex, isToggle?: boolean`  
**Return:** `void`  
**Description:** Selects cell by row/column index

```typescript
@Component({
  selector: 'app-grid-selectcell',
  standalone: true,
  imports: [GridModule],
  template: ``
})
export class GridSelectCellComponent {
  @ViewChild('grid') gridComponent!: GridComponent;

  handleSelectCell(): void {
    this.gridComponent.selectCell({ rowIndex: 0, cellIndex: 1 });
  }
}
```

---

## Column Management Methods

### getColumns()
**Parameters:** `isRefresh?: boolean`  
**Return:** `Column[]`  
**Description:** Gets column definitions

```typescript
@Component({
  selector: 'app-grid-getcolumns',
  standalone: true,
  imports: [GridModule],
  template: ``
})
export class GridGetColumnsComponent {
  @ViewChild('grid') gridComponent!: GridComponent;

  getGridColumns(): void {
    const columns = this.gridComponent.getColumns();
    console.log('Grid columns:', columns);
  }
}
```

### getColumnByField()
**Parameters:** `field: string`  
**Return:** `Column`  
**Description:** Gets column by field name

```typescript
@Component({
  selector: 'app-grid-getcolumnbyfield',
  standalone: true,
  imports: [GridModule],
  template: ``
})
export class GridGetColumnByFieldComponent {
  @ViewChild('grid') gridComponent!: GridComponent;

  getColumn(): void {
    const column = this.gridComponent.getColumnByField('CustomerID');
    console.log('Column:', column);
  }
}
```

### getColumnFieldNames()
**Return:** `string[]`  
**Description:** Gets all column field names

```typescript
@Component({
  selector: 'app-grid-getfieldnames',
  standalone: true,
  imports: [GridModule],
  template: ``
})
export class GridGetFieldNamesComponent {
  @ViewChild('grid') gridComponent!: GridComponent;

  getFieldNames(): void {
    const fields = this.gridComponent.getColumnFieldNames();
    console.log('Field names:', fields);
  }
}
```

### hideColumns()
**Parameters:** `keys: string | string[], hideBy?: string`  
**Return:** `void`  
**Description:** Hides columns by name

```typescript
@Component({
  selector: 'app-grid-hidecolumns',
  standalone: true,
  imports: [GridModule],
  template: ``
})
export class GridHideColumnsComponent {
  @ViewChild('grid') gridComponent!: GridComponent;

  handleHideColumns(): void {
    this.gridComponent.hideColumns('Freight');  // Hide single column
    // Or multiple
    this.gridComponent.hideColumns(['Freight', 'ShipCity']);
  }
}
```

### showColumns()
**Parameters:** `keys: string | string[], showBy?: string`  
**Return:** `void`  
**Description:** Shows columns by name

```typescript
@Component({
  selector: 'app-grid-showcolumns',
  standalone: true,
  imports: [GridModule],
  template: ``
})
export class GridShowColumnsComponent {
  @ViewChild('grid') gridComponent!: GridComponent;

  handleShowColumns(): void {
    this.gridComponent.showColumns('Freight');
  }
}
```

### autoFitColumns()
**Parameters:** `fieldNames?: string | string[]`  
**Return:** `void`  
**Description:** Auto-fits columns to content

```typescript
@Component({
  selector: 'app-grid-autofitcolumns',
  standalone: true,
  imports: [GridModule],
  template: ``
})
export class GridAutoFitColumnsComponent {
  @ViewChild('grid') gridComponent!: GridComponent;

  handleAutoFit(): void {
    this.gridComponent.autoFitColumns();  // Auto-fit all columns
  }
}
```

### reorderColumns()
**Parameters:** `fromFName: string | string[], toFName: string`  
**Return:** `void`  
**Description:** Reorders columns by field names

```typescript
@Component({
  selector: 'app-grid-reordercolumns',
  standalone: true,
  imports: [GridModule],
  template: ``
})
export class GridReorderColumnsComponent {
  @ViewChild('grid') gridComponent!: GridComponent;

  handleReorderColumns(): void {
    this.gridComponent.reorderColumns('OrderID', 'Freight');
  }
}
```

---

## Navigation & Paging Methods

### goToPage()
**Parameters:** `pageNo: number`  
**Return:** `void`  
**Description:** Navigates to specified page

```typescript
@Component({
  selector: 'app-grid-gotopage',
  standalone: true,
  imports: [GridModule],
  template: ``
})
export class GridGoToPageComponent {
  @ViewChild('grid') gridComponent!: GridComponent;

  handleGoToPage(): void {
    this.gridComponent.goToPage(3);  // Go to page 3
  }
}
```

### search()
**Parameters:** `searchString: string`  
**Return:** `void`  
**Description:** Searches records with key

```typescript
@Component({
  selector: 'app-grid-search',
  standalone: true,
  imports: [GridModule],
  template: ``
})
export class GridSearchComponent {
  @ViewChild('grid') gridComponent!: GridComponent;

  handleSearch(searchValue: string): void {
    this.gridComponent.search(searchValue);
  }
}
```

### sortColumn()
**Parameters:** `columnName: string, direction: SortDirection, isMultiSort?: boolean`  
**Return:** `void`  
**Description:** Sorts column with direction (Ascending or Descending)

```typescript
@Component({
  selector: 'app-grid-sortcolumn',
  standalone: true,
  imports: [GridModule],
  template: ``
})
export class GridSortColumnComponent {
  @ViewChild('grid') gridComponent!: GridComponent;

  handleSortColumn(): void {
    this.gridComponent.sortColumn('Freight', 'Ascending');
  }
}
```

### clearSorting()
**Return:** `void`  
**Description:** Clears all sorting

```typescript
@Component({
  selector: 'app-grid-clearsorting',
  standalone: true,
  imports: [GridModule],
  template: ``
})
export class GridClearSortingComponent {
  @ViewChild('grid') gridComponent!: GridComponent;

  handleClearSorting(): void {
    this.gridComponent.clearSorting();
  }
}
```

---

## Filtering Methods

### filterByColumn()
**Parameters:** `fieldName: string, filterOperator: string, filterValue: string | number | Date | boolean`  
**Return:** `void`  
**Description:** Filters by column with operators

```typescript
@Component({
  selector: 'app-grid-filterbycolumn',
  standalone: true,
  imports: [GridModule],
  template: ``
})
export class GridFilterByColumnComponent {
  @ViewChild('grid') gridComponent!: GridComponent;

  handleFilterByColumn(): void {
    // Filter Freight > 100
    this.gridComponent.filterByColumn('Freight', 'greaterThan', 100);
  }
}
```

### clearFiltering()
**Parameters:** `fields?: string[]`  
**Return:** `void`  
**Description:** Clears all filters or specific fields

```typescript
@Component({
  selector: 'app-grid-clearfiltering',
  standalone: true,
  imports: [GridModule],
  template: ``
})
export class GridClearFilteringComponent {
  @ViewChild('grid') gridComponent!: GridComponent;

  handleClearFilters(): void {
    this.gridComponent.clearFiltering();
  }
}
```

### getFilteredRecords()
**Return:** `Object[] | Promise`  
**Description:** Gets all filtered records

```typescript
@Component({
  selector: 'app-grid-getfiltered',
  standalone: true,
  imports: [GridModule],
  template: ``
})
export class GridGetFilteredComponent {
  @ViewChild('grid') gridComponent!: GridComponent;

  getFiltered(): void {
    const filtered = this.gridComponent.getFilteredRecords();
    console.log('Filtered records:', filtered);
  }
}
```

---

## Export Methods

### excelExport()
**Parameters:** `excelExportProperties?: ExcelExportProperties`  
**Return:** `Promise`  
**Description:** Exports grid to Excel file (.xlsx)

```typescript
@Component({
  selector: 'app-grid-excelexport-method',
  standalone: true,
  imports: [GridModule],
  providers: [ExcelExportService],
  template: ``
})
export class GridExcelExportMethodComponent {
  @ViewChild('grid') gridComponent!: GridComponent;

  async handleExcelExport(): Promise<void> {
    await this.gridComponent.excelExport();
  }
}
```

### csvExport()
**Parameters:** `excelExportProperties?: ExcelExportProperties`  
**Return:** `Promise`  
**Description:** Exports grid to CSV file

```typescript
@Component({
  selector: 'app-grid-csvexport',
  standalone: true,
  imports: [GridModule],
  providers: [ExcelExportService],
  template: ``
})
export class GridCsvExportComponent {
  @ViewChild('grid') gridComponent!: GridComponent;

  async handleCsvExport(): Promise<void> {
    await this.gridComponent.csvExport();
  }
}
```

### pdfExport()
**Parameters:** `pdfExportProperties?: PdfExportProperties`  
**Return:** `Promise`  
**Description:** Exports grid to PDF document

```typescript
@Component({
  selector: 'app-grid-pdfexport-method',
  standalone: true,
  imports: [GridModule],
  providers: [PdfExportService],
  template: ``
})
export class GridPdfExportMethodComponent {
  @ViewChild('grid') gridComponent!: GridComponent;

  async handlePdfExport(): Promise<void> {
    await this.gridComponent.pdfExport();
  }
}
```

### print()
**Return:** `void`  
**Description:** Prints all pages

```typescript
@Component({
  selector: 'app-grid-print',
  standalone: true,
  imports: [GridModule],
  template: ``
})
export class GridPrintComponent {
  @ViewChild('grid') gridComponent!: GridComponent;

  handlePrint(): void {
    this.gridComponent.print();
  }
}
```

---

## Events Overview

The Grid component provides 65+ events. Here are the most commonly used ones.

---

## Data Events

### beforeDataBound
**Arguments:** `BeforeDataBoundArgs`  
**Description:** Triggers before data is bound to grid

```typescript
@Component({
  selector: 'app-grid-beforedatabound',
  standalone: true,
  imports: [GridModule],
  template: `
    <ejs-grid [dataSource]="data" (beforeDataBound)="onBeforeDataBound($event)">
      <e-columns>
        <e-column field="OrderID" headerText="Order ID" width="100"></e-column>
      </e-columns>
    </ejs-grid>
  `
})
export class GridBeforeDataBoundComponent {
  data: any[] = [];

  onBeforeDataBound(args: any): void {
    console.log('Before data binding:', args);
  }
}
```

### dataBound
**Arguments:** `Object`  
**Description:** Triggers when data source is populated

```typescript
@Component({
  selector: 'app-grid-databound',
  standalone: true,
  imports: [GridModule],
  template: `
    <ejs-grid [dataSource]="data" (dataBound)="onDataBound($event)">
      <e-columns>
        <e-column field="OrderID" headerText="Order ID" width="100"></e-column>
      </e-columns>
    </ejs-grid>
  `
})
export class GridDataBoundComponent {
  data: any[] = [];

  onDataBound(args: any): void {
    console.log('Data bound completed');
  }
}
```

### dataSourceChanged
**Arguments:** `DataSourceChangedEventArgs`  
**Description:** Triggers when grid data is added/deleted/updated

```typescript
@Component({
  selector: 'app-grid-datasourcechanged',
  standalone: true,
  imports: [GridModule],
  template: `
    <ejs-grid [dataSource]="data" (dataSourceChanged)="onDataSourceChanged($event)">
      <e-columns>
        <e-column field="OrderID" headerText="Order ID" width="100"></e-column>
      </e-columns>
    </ejs-grid>
  `
})
export class GridDataSourceChangedComponent {
  data: any[] = [];

  onDataSourceChanged(args: any): void {
    console.log('Data source changed:', args.action);
    console.log('Data:', args.data);
  }
}
```

---

## Interaction Events

### rowSelecting
**Arguments:** `RowSelectingEventArgs`  
**Description:** Triggers before row selection

```typescript
@Component({
  selector: 'app-grid-rowselecting',
  standalone: true,
  imports: [GridModule],
  template: `
    <ejs-grid [dataSource]="data" (rowSelecting)="onRowSelecting($event)">
      <e-columns>
        <e-column field="OrderID" headerText="Order ID" width="100"></e-column>
      </e-columns>
    </ejs-grid>
  `
})
export class GridRowSelectingComponent {
  data: any[] = [];

  onRowSelecting(args: any): void {
    console.log('Row index:', args.rowIndex);
    if (args.rowIndex === 0) {
      args.cancel = true;  // Prevent selection
    }
  }
}
```

### rowSelected
**Arguments:** `RowSelectedEventArgs`  
**Description:** Triggers after row selection

```typescript
@Component({
  selector: 'app-grid-rowselected',
  standalone: true,
  imports: [GridModule],
  template: `
    <ejs-grid [dataSource]="data" (rowSelected)="onRowSelected($event)">
      <e-columns>
        <e-column field="OrderID" headerText="Order ID" width="100"></e-column>
      </e-columns>
    </ejs-grid>
  `
})
export class GridRowSelectedComponent {
  data: any[] = [];

  onRowSelected(args: any): void {
    console.log('Selected row data:', args.data);
  }
}
```

### rowDeselecting
**Arguments:** `RowDeselectingEventArgs`  
**Description:** Triggers before row deselection

```typescript
@Component({
  selector: 'app-grid-rowdeselecting',
  standalone: true,
  imports: [GridModule],
  template: `
    <ejs-grid [dataSource]="data" (rowDeselecting)="onRowDeselecting($event)">
      <e-columns>
        <e-column field="OrderID" headerText="Order ID" width="100"></e-column>
      </e-columns>
    </ejs-grid>
  `
})
export class GridRowDeselectingComponent {
  data: any[] = [];

  onRowDeselecting(args: any): void {
    console.log('Deselecting row:', args.rowIndex);
  }
}
```

### rowDeselected
**Arguments:** `RowDeselectedEventArgs`  
**Description:** Triggers after row deselection

```typescript
@Component({
  selector: 'app-grid-rowdeselected',
  standalone: true,
  imports: [GridModule],
  template: `
    <ejs-grid [dataSource]="data" (rowDeselected)="onRowDeselected($event)">
      <e-columns>
        <e-column field="OrderID" headerText="Order ID" width="100"></e-column>
      </e-columns>
    </ejs-grid>
  `
})
export class GridRowDeselectedComponent {
  data: any[] = [];

  onRowDeselected(args: any): void {
    console.log('Deselected row data:', args.data);
  }
}
```

### cellSelecting
**Arguments:** `CellSelectingEventArgs`  
**Description:** Triggers before cell selection

```typescript
@Component({
  selector: 'app-grid-cellselecting',
  standalone: true,
  imports: [GridModule],
  template: `
    <ejs-grid [dataSource]="data" (cellSelecting)="onCellSelecting($event)">
      <e-columns>
        <e-column field="OrderID" headerText="Order ID" width="100"></e-column>
      </e-columns>
    </ejs-grid>
  `
})
export class GridCellSelectingComponent {
  data: any[] = [];

  onCellSelecting(args: any): void {
    console.log('Selecting cell at:', args.cellIndex);
  }
}
```

### cellSelected
**Arguments:** `CellSelectedEventArgs`  
**Description:** Triggers after cell selection

```typescript
@Component({
  selector: 'app-grid-cellselected',
  standalone: true,
  imports: [GridModule],
  template: `
    <ejs-grid [dataSource]="data" (cellSelected)="onCellSelected($event)">
      <e-columns>
        <e-column field="OrderID" headerText="Order ID" width="100"></e-column>
      </e-columns>
    </ejs-grid>
  `
})
export class GridCellSelectedComponent {
  data: any[] = [];

  onCellSelected(args: any): void {
    console.log('Selected cell value:', args.value);
  }
}
```

---

## Editing Events

### actionBegin
**Arguments:** `ActionEventArgs`  
**Description:** Triggers before grid action begins (add, edit, delete, sort, filter, etc.)

```typescript
@Component({
  selector: 'app-grid-actionbegin',
  standalone: true,
  imports: [GridModule],
  template: `
    <ejs-grid [dataSource]="data" (actionBegin)="onActionBegin($event)">
      <e-columns>
        <e-column field="OrderID" headerText="Order ID" width="100"></e-column>
      </e-columns>
    </ejs-grid>
  `
})
export class GridActionBeginComponent {
  data: any[] = [];

  onActionBegin(args: any): void {
    console.log('Action type:', args.requestType);
    // requestType can be: 'save', 'cancel', 'delete', 'add', etc.
  }
}
```

### actionComplete
**Arguments:** `ActionEventArgs`  
**Description:** Triggers after grid action completes

```typescript
@Component({
  selector: 'app-grid-actioncomplete',
  standalone: true,
  imports: [GridModule],
  template: `
    <ejs-grid [dataSource]="data" (actionComplete)="onActionComplete($event)">
      <e-columns>
        <e-column field="OrderID" headerText="Order ID" width="100"></e-column>
      </e-columns>
    </ejs-grid>
  `
})
export class GridActionCompleteComponent {
  data: any[] = [];

  onActionComplete(args: any): void {
    if (args.requestType === 'save') {
      console.log('Save completed');
      console.log('New data:', args.data);
    }
  }
}
```

### actionFailure
**Arguments:** `FailureEventArgs`  
**Description:** Triggers when grid action fails

```typescript
@Component({
  selector: 'app-grid-actionfailure',
  standalone: true,
  imports: [GridModule],
  template: `
    <ejs-grid [dataSource]="data" (actionFailure)="onActionFailure($event)">
      <e-columns>
        <e-column field="OrderID" headerText="Order ID" width="100"></e-column>
      </e-columns>
    </ejs-grid>
  `
})
export class GridActionFailureComponent {
  data: any[] = [];

  onActionFailure(args: any): void {
    console.error('Action failed:', args.error);
  }
}
```

### beginEdit
**Arguments:** `BeginEditEventArgs`  
**Description:** Triggers after entering edit mode

```typescript
@Component({
  selector: 'app-grid-beginedit',
  standalone: true,
  imports: [GridModule],
  template: `
    <ejs-grid [dataSource]="data" (beginEdit)="onBeginEdit($event)">
      <e-columns>
        <e-column field="OrderID" headerText="Order ID" width="100"></e-column>
      </e-columns>
    </ejs-grid>
  `
})
export class GridBeginEditComponent {
  data: any[] = [];

  onBeginEdit(args: any): void {
    console.log('Now in edit mode for:', args.data);
  }
}
```

---

## Query Cell Info Event

### queryCellInfo
**Arguments:** `QueryCellInfoEventArgs`  
**Description:** Triggers for each cell rendering

```typescript
@Component({
  selector: 'app-grid-querycellinfo',
  standalone: true,
  imports: [GridModule],
  template: `
    <ejs-grid [dataSource]="data" (queryCellInfo)="onQueryCellInfo($event)">
      <e-columns>
        <e-column field="OrderID" headerText="Order ID" width="100"></e-column>
      </e-columns>
    </ejs-grid>
  `
})
export class GridQueryCellInfoComponent {
  data: any[] = [];

  onQueryCellInfo(args: any): void {
    // Highlight cells based on value
    if (args.cell?.index === 0 && args.data?.Freight > 100) {
      args.cell.classList.add('high-freight');
    }
  }
}
```

---

## Column Events

### columnSelecting
**Arguments:** `ColumnSelectingEventArgs`  
**Description:** Triggers before column selection

```typescript
@Component({
  selector: 'app-grid-columnselecting',
  standalone: true,
  imports: [GridModule],
  template: `
    <ejs-grid [dataSource]="data" (columnSelecting)="onColumnSelecting($event)">
      <e-columns>
        <e-column field="OrderID" headerText="Order ID" width="100"></e-column>
      </e-columns>
    </ejs-grid>
  `
})
export class GridColumnSelectingComponent {
  data: any[] = [];

  onColumnSelecting(args: any): void {
    console.log('Selecting column:', args.column?.field);
  }
}
```

### columnSelected
**Arguments:** `ColumnSelectedEventArgs`  
**Description:** Triggers after column selection

```typescript
@Component({
  selector: 'app-grid-columnselected',
  standalone: true,
  imports: [GridModule],
  template: `
    <ejs-grid [dataSource]="data" (columnSelected)="onColumnSelected($event)">
      <e-columns>
        <e-column field="OrderID" headerText="Order ID" width="100"></e-column>
      </e-columns>
    </ejs-grid>
  `
})
export class GridColumnSelectedComponent {
  data: any[] = [];

  onColumnSelected(args: any): void {
    console.log('Selected column:', args.column?.field);
  }
}
```

---

## Practical Examples

### Complete CRUD Example

```typescript
import { Component, ViewChild } from '@angular/core';
import { CommonModule } from '@angular/common';
import { GridComponent, GridModule, PageService, SortService, FilterService, EditService, ToolbarService } from '@syncfusion/ej2-angular-grids';

@Component({
  selector: 'app-grid-crud',
  standalone: true,
  imports: [GridModule, CommonModule],
  providers: [PageService, SortService, FilterService, EditService, ToolbarService],
  template: `
    <div>
      <div style="margin-bottom: 10px;">
        <button (click)="handleAddRecord()">Add New Record</button>
        <button (click)="handleUpdateSelected()">Update Selected</button>
        <button (click)="handleDeleteSelected()">Delete Selected</button>
      </div>

      <ejs-grid #grid
        [dataSource]="data"
        [allowPaging]="true"
        [allowSorting]="true"
        [allowFiltering]="true"
        [editSettings]="editSettings"
        [selectionSettings]="selectionSettings"
      >
        <e-columns>
          <e-column field="OrderID" headerText="Order ID" width="100" [isPrimaryKey]="true"></e-column>
          <e-column field="CustomerID" headerText="Customer" width="120"></e-column>
          <e-column field="Freight" headerText="Freight" width="100" format="C2"></e-column>
          <e-column field="ShipCity" headerText="Ship City" width="120"></e-column>
        </e-columns>
      </ejs-grid>
    </div>
  `
})
export class GridCRUDExample {
  @ViewChild('grid') gridComponent!: GridComponent;

  data = [
    { OrderID: 10001, CustomerID: 'ALFKI', Freight: 32.38, ShipCity: 'Berlin' },
    { OrderID: 10002, CustomerID: 'ANATR', Freight: 11.61, ShipCity: 'Madrid' },
    { OrderID: 10003, CustomerID: 'ANTON', Freight: 65.10, ShipCity: 'Marseille' },
  ];

  editSettings = {
    allowAdding: true,
    allowEditing: true,
    allowDeleting: true,
    mode: 'Dialog'
  };

  selectionSettings = {
    type: 'Single',
    mode: 'Row'
  };

  handleAddRecord(): void {
    const newRecord = {
      OrderID: Math.max(...this.data.map(d => d.OrderID)) + 1,
      CustomerID: 'NEWCUST',
      Freight: 50.00,
      ShipCity: 'Paris'
    };
    this.gridComponent.addRecord(newRecord);
  }

  handleUpdateSelected(): void {
    const selected = this.gridComponent.getSelectedRecords();
    if (selected.length > 0) {
      this.gridComponent.setCellValue(selected[0].OrderID, 'Freight', 99.99);
    }
  }

  handleDeleteSelected(): void {
    const selected = this.gridComponent.getSelectedRecords();
    if (selected.length > 0) {
      this.gridComponent.deleteRecord('OrderID', selected[0]);
    }
  }
}
```

### Grid with Custom Events

```typescript
@Component({
  selector: 'app-grid-events',
  standalone: true,
  imports: [GridModule],
  providers: [PageService, EditService],
  template: `
    <ejs-grid #grid
      [dataSource]="data"
      [allowPaging]="true"
      [editSettings]="editSettings"
      (actionBegin)="onActionBegin($event)"
      (actionComplete)="onActionComplete($event)"
      (rowSelected)="onRowSelected($event)"
      (queryCellInfo)="onQueryCellInfo($event)"
    >
      <e-columns>
        <e-column field="OrderID" headerText="Order ID" width="100"></e-column>
        <e-column field="CustomerID" headerText="Customer" width="120"></e-column>
        <e-column field="Freight" headerText="Freight" width="100"></e-column>
      </e-columns>
    </ejs-grid>
  `
})
export class GridWithEvents {
  @ViewChild('grid') gridComponent!: GridComponent;

  data: any[] = [];
  editSettings = { allowEditing: true, allowAdding: true, allowDeleting: true };

  onActionBegin(args: any): void {
    console.log(`Action started: ${args.requestType}`);
  }

  onActionComplete(args: any): void {
    console.log(`Action completed: ${args.requestType}`);
    if (args.requestType === 'save') {
      console.log('Data saved:', args.data);
    }
  }

  onRowSelected(args: any): void {
    console.log('Row selected:', args.data);
  }

  onQueryCellInfo(args: any): void {
    if (args.column?.field === 'Freight' && args.data?.Freight > 50) {
      args.cell.style.backgroundColor = '#ffcccc';
    }
  }
}
```

### Dynamic Column Management

```typescript
@Component({
  selector: 'app-grid-dynamic-columns',
  standalone: true,
  imports: [GridModule, CommonModule],
  template: `
    <div>
      <div style="margin-bottom: 10px;">
        <button (click)="handleHideFreight()">Hide Freight</button>
        <button (click)="handleShowFreight()">Show Freight</button>
        <button (click)="handleAutoFit()">Auto Fit</button>
        <button (click)="handleReorder()">Reorder Columns</button>
      </div>

      <ejs-grid #grid [dataSource]="data">
        <e-columns>
          <e-column field="OrderID" headerText="Order ID" width="100"></e-column>
          <e-column field="CustomerID" headerText="Customer" width="120"></e-column>
          <e-column field="Freight" headerText="Freight" width="100"></e-column>
          <e-column field="ShipCity" headerText="City" width="120"></e-column>
        </e-columns>
      </ejs-grid>
    </div>
  `
})
export class GridWithDynamicColumns {
  @ViewChild('grid') gridComponent!: GridComponent;

  data = [
    { OrderID: 10001, CustomerID: 'ALFKI', Freight: 32.38, ShipCity: 'Berlin' },
    { OrderID: 10002, CustomerID: 'ANATR', Freight: 11.61, ShipCity: 'Madrid' },
    { OrderID: 10003, CustomerID: 'ANTON', Freight: 65.10, ShipCity: 'Paris' }
  ];

  handleHideFreight(): void {
    this.gridComponent.hideColumns('Freight');
  }

  handleShowFreight(): void {
    this.gridComponent.showColumns('Freight');
  }

  handleAutoFit(): void {
    this.gridComponent.autoFitColumns();
  }

  handleReorder(): void {
    this.gridComponent.reorderColumns('OrderID', 'Freight');
  }
}
```

---

## Common Operator Values for Filtering

| Operator | Description | Example |
|----------|-------------|---------|
| `equal` | Equals | `'equal'` |
| `notequal` | Not equals | `'notequal'` |
| `contains` | Contains | `'contains'` |
| `startswith` | Starts with | `'startswith'` |
| `endswith` | Ends with | `'endswith'` |
| `greaterthan` | Greater than | `'greaterthan'` |
| `lessthan` | Less than | `'lessthan'` |
| `greaterthanorequal` | Greater than or equal | `'greaterthanorequal'` |
| `lessthanorequal` | Less than or equal | `'lessthanorequal'` |

---

## Sort Direction Values

| Value | Description |
|-------|-------------|
| `Ascending` | Ascending sort order |
| `Descending` | Descending sort order |

---

## Common Patterns

### Check if Grid has data
```tsx
const hasData = gridRef.current.currentViewData?.length > 0;
```

### Get total records count
```tsx
const totalRecords = gridRef.current.pageSettings?.totalRecordsCount || data.length;
```

### Refresh grid after external data change
```tsx
gridRef.current.refresh();
```

### Get current page records
```tsx
const currentPageRecords = gridRef.current.getCurrentViewRecords();
```

