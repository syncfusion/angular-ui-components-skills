# Selection in Angular Gantt Chart

## Table of Contents
- [Enable / Disable Selection](#enable--disable-selection)
- [Selection Mode](#selection-mode)
- [Selection Type](#selection-type)
- [Toggle Selection](#toggle-selection)
- [Row Selection Configuration](#row-selection-configuration)
- [Cell Selection Configuration](#cell-selection-configuration)
- [Handle Selection Events](#handle-selection-events)
- [Programmatic Selection](#programmatic-selection)

---

## Enable / Disable Selection

Selection is enabled by default. Inject `SelectionService` to activate:

```typescript
@Component({
  providers: [SelectionService],
  template: `<ejs-gantt [allowSelection]="true"></ejs-gantt>`
})
```

Disable entirely:
```html
<ejs-gantt [allowSelection]="false"></ejs-gantt>
```

---

## Selection Mode

Configure which elements can be selected via `selectionSettings.mode`:

```typescript
public selectionSettings: object = {
  mode: 'Row'   // 'Row' (default) | 'Cell' | 'Both'
};
```

| Mode | Behavior |
|------|----------|
| `'Row'` | Selects entire rows (default) |
| `'Cell'` | Selects individual cells |
| `'Both'` | Rows and cells selectable simultaneously |

---

## Selection Type

Control how many rows/cells can be selected at once:

```typescript
public selectionSettings: object = {
  type: 'Multiple'   // 'Single' (default) | 'Multiple'
};
```

- **Single:** One row/cell at a time
- **Multiple:** Hold **Ctrl** (Windows) or **Cmd** (Mac) to select multiple rows/cells

---

## Toggle Selection

Allow deselecting by clicking an already-selected row:

```typescript
public selectionSettings: object = {
  enableToggle: true   // Default: false
};
```

---

## Row Selection Configuration

```typescript
public selectionSettings: object = {
  mode: 'Row',
  type: 'Multiple',
  enableToggle: true,
  persistSelection: false,  // Keep selection after data refresh
  checkboxOnly: false        // Selection only via checkbox column
};
```

---

## Cell Selection Configuration

```typescript
public selectionSettings: object = {
  mode: 'Cell',
  type: 'Multiple',
  cellSelectionMode: 'Box'   // 'Flow' | 'Box' | 'BoxWithBorder'
};
```

- `'Flow'`: Selects all cells between start and end
- `'Box'`: Selects a rectangular block of cells
- `'BoxWithBorder'`: Box selection with visible border

---

## Handle Selection Events

```typescript
public rowSelected(args: any): void {
  console.log('Selected row data:', args.data);
  console.log('Row index:', args.rowIndex);
}

public rowDeselected(args: any): void {
  console.log('Deselected row:', args.data);
}

public cellSelected(args: any): void {
  console.log('Selected cell field:', args.cellIndex.cellIndex);
  console.log('Cell value:', args.currentCell.innerText);
}
```

```html
<ejs-gantt
  (rowSelected)="rowSelected($event)"
  (rowDeselected)="rowDeselected($event)"
  (cellSelected)="cellSelected($event)">
</ejs-gantt>
```

---

## Programmatic Selection

```typescript
// Select a specific row by index
this.ganttObj.selectionModule.selectRow(2);

// Select multiple rows
this.ganttObj.selectionModule.selectRows([0, 2, 4]);

// Select a cell
this.ganttObj.selectionModule.selectCell({ rowIndex: 1, cellIndex: 2 });

// Clear selection
this.ganttObj.selectionModule.clearSelection();

// Get selected row data
const selectedRecords = this.ganttObj.selectionModule.getSelectedRecords();
```
