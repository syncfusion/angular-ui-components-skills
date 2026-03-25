# Rows in Angular Gantt Chart

## Table of Contents
- [Row Style Customization](#row-style-customization)
- [Row Template](#row-template)
- [Row Height](#row-height)
- [Row Spanning](#row-spanning)
- [Drag and Drop Row Reordering](#drag-and-drop-row-reordering)
- [Indent and Outdent](#indent-and-outdent)
- [Alternate Row Styling](#alternate-row-styling)

---

## Row Style Customization

### Via Event

Use `rowDataBound` to apply dynamic styles based on task data:

```typescript
public rowDataBound(args: any): void {
  if (args.data.Progress > 80) {
    args.row.style.backgroundColor = '#e8f5e9';  // Green tint for high progress
  }
  if (args.data.Priority === 'High') {
    args.row.classList.add('high-priority-row');
  }
}
```

```html
<ejs-gantt (rowDataBound)="rowDataBound($event)"></ejs-gantt>
```

### Via queryCellInfo (Cell Level)

```typescript
public queryCellInfo(args: any): void {
  if (args.column.field === 'Progress' && args.data.Progress < 30) {
    args.cell.style.color = 'red';
    args.cell.style.fontWeight = 'bold';
  }
}
```

### Via CSS

Target built-in classes:
```css
/* Selected row */
.e-gantt .e-selectionbackground {
  background-color: #f9920b !important;
}

/* Alternate rows */
.e-gantt .e-altrow {
  background-color: #f5f5f5;
}
```

---

## Row Template

Customize the entire row layout in the TreeGrid portion:

```typescript
@Component({
  template: `
    <ejs-gantt [rowTemplate]="'#rowTemplate'">
      <ng-template #rowTemplate let-data>
        <tr>
          <td>{{ data.TaskID }}</td>
          <td><b>{{ data.TaskName }}</b></td>
          <td>{{ data.StartDate | date:'MM/dd' }}</td>
        </tr>
      </ng-template>
    </ejs-gantt>
  `
})
```

---

## Row Height

Set uniform row height for all rows:

```html
<ejs-gantt [rowHeight]="50"></ejs-gantt>
```

`rowHeight` must exceed `taskbarHeight`. Default is 36px.

---

## Row Spanning

Merge cells vertically across multiple rows in the TreeGrid:

```typescript
public queryRowInfo(args: any): void {
  if (args.data.TaskID === 1) {
    args.rowSpan = 3;  // Span this row across 3 rows
  }
}
```

```html
<ejs-gantt (queryRowInfo)="queryRowInfo($event)"></ejs-gantt>
```

---

## Drag and Drop Row Reordering

Allow users to drag rows to reorder or reparent tasks:

```html
<ejs-gantt [allowRowDragAndDrop]="true"></ejs-gantt>
```

Handle events:
```typescript
public rowDragStartHelper(args: any): void {
  // Prevent dragging milestone tasks
  if (args.data[0].Duration === 0) {
    args.cancel = true;
  }
}

public rowDrop(args: any): void {
  console.log('Dropped task:', args.data);
  console.log('Target row index:', args.dropIndex);
  console.log('Drop position:', args.dropPosition);  // 'Above' | 'Below' | 'Child'
}
```

---

## Indent and Outdent

Change task hierarchy level via toolbar buttons or programmatically:

```typescript
public toolbar: string[] = ['Indent', 'Outdent'];
```

```typescript
// Programmatic
this.ganttObj.indent();   // Make selected task a child of the task above
this.ganttObj.outdent();  // Promote task up one level in hierarchy
```

Requirements: `EditService` must be injected. Select a row before indenting/outdenting.

**Indent rules:**
- Task becomes a child of the immediately preceding sibling
- Cannot indent the first task (no preceding sibling)
- Cannot outdent root-level tasks

---

## Alternate Row Styling

Enable/disable alternate row coloring:

```html
<ejs-gantt [enableAltRow]="true"></ejs-gantt>
```

Default is `true`. Customize the alternate row color via CSS:
```css
.e-gantt .e-altrow {
  background-color: #f0f4f8;
}
```
