---
name: syncfusion-angular-multi-select
description: Implement the Syncfusion Angular MultiSelect Dropdown component (ejs-multiselect) for multi-value selection with checkbox support, tag/chip inputs, and searchable lists. Use this when building multi-select dropdowns, checkbox selection lists, or tag-based input fields. This skill covers filtering, CheckBoxSelection mode, cascading dropdowns, and data binding for the MultiSelect component.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
  category: "Dropdowns"
---

# Implementing the Syncfusion Angular MultiSelect Dropdown

The MultiSelect Dropdown (`ejs-multiselect`) allows users to select one or more values from a predefined list. It supports four visual modes (Default/Box/Delimiter/CheckBox), rich filtering, templates, grouping, remote data, virtualization, and full Angular form integration.

## Component Overview

| Mode | Behavior | Use When |
|------|----------|----------|
| `Default` | Selected items shown as chips in input | Standard multi-select |
| `Box` | Same as Default, explicit box display | Visual clarity needed |
| `Delimiter` | Selected items as comma-separated text | Space-constrained layouts |
| `CheckBox` | Popup shows checkboxes per item | Bulk selection workflows |

## Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installation and package setup (`ng add`)
- CSS imports and theme configuration
- Basic `<ejs-multiselect>` in standalone Angular component
- Popup height/width configuration
- Angular version notes (standalone default in Angular 19+)

### Data Binding & Value Binding
📄 **Read:** [references/data-binding.md](references/data-binding.md)
- Local arrays (strings, numbers, objects)
- `fields` mapping: `text`, `value`, `groupBy`, `iconCss`, `disabled`
- Remote data with DataManager (OData, Web API)
- Pre-selecting values programmatically
- Object binding with `allowObjectBinding`

### Selection Modes, Chips & Item Control
📄 **Read:** [references/selection-modes.md](references/selection-modes.md)
- `mode` property: Default / Box / Delimiter / CheckBox
- CheckBox mode: `CheckBoxSelection`, `showSelectAll`, `maximumSelectionLength`
- Chip customization via `tagging` event
- Custom values with `allowCustomValue`
- Disabling specific items: `fields.disabled`, `disableItem()`
- `addTagOnBlur` and `changeOnBlur` behaviors

### Filtering
📄 **Read:** [references/filtering.md](references/filtering.md)
- Enabling `allowFiltering` + `filtering` event + `updateData()`
- `filterType`: `startsWith`, `contains`, `endsWith`
- Minimum character threshold before query fires
- Remote filtering with DataManager
- Case-insensitive matching

### Templates
📄 **Read:** [references/templates.md](references/templates.md)
- `itemTemplate` — custom list item rendering with `ng-template`
- `valueTemplate` — customize chip/selected value display
- `groupTemplate` — custom group header content
- `headerTemplate` / `footerTemplate` — popup top/bottom
- `noRecordsTemplate` / `actionFailureTemplate`

### Grouping & Cascading
📄 **Read:** [references/grouping-and-items.md](references/grouping-and-items.md)
- `fields.groupBy` — organize items into categories
- Fixed vs inline group headers
- `enableGroupCheckBox` for group-level Select All
- Cascading MultiSelect: parent `change` event → filter child data
- Country → State → City cascade pattern

### Form Integration
📄 **Read:** [references/form-support.md](references/form-support.md)
- Template-driven forms: `FormsModule`, `ngModel`, two-way binding
- Reactive forms: `ReactiveFormsModule`, `FormGroup`, `FormControl`
- `formControlName` usage and validation
- Required field and custom validator patterns

### Advanced Features
📄 **Read:** [references/advanced-features.md](references/advanced-features.md)
- Virtualization for large datasets (`enableVirtualization`)
- Popup resize (`allowResize`, resize events)
- Icons in list items (`iconCss` field)
- Localization (`L10n.load()`, locale key overrides)
- Accessibility: WAI-ARIA, keyboard navigation, WCAG 2.2
- RTL support

### API Reference
📄 **Read:** [references/api.md](references/api.md)
- Complete list of all properties with types and defaults
- All public methods with signatures, parameters, and return types
- All events with argument types and usage examples
- Module injection notes (`CheckBoxSelectionService`)

## Quick Start

```bash
ng add @syncfusion/ej2-angular-dropdowns
```

```typescript
// app.component.ts — Standalone Angular 19+
import { Component } from '@angular/core';
import { MultiSelectModule } from '@syncfusion/ej2-angular-dropdowns';

@Component({
  standalone: true,
  imports: [MultiSelectModule],
  selector: 'app-root',
  template: `
    <ejs-multiselect
      [dataSource]="skills"
      [fields]="fields"
      placeholder="Select skills"
      [(value)]="selectedSkills"
      (change)="onChange($event)">
    </ejs-multiselect>
  `
})
export class AppComponent {
  public skills = [
    { id: 1, name: 'Angular' },
    { id: 2, name: 'React' },
    { id: 3, name: 'TypeScript' },
    { id: 4, name: 'Node.js' },
  ];
  public fields = { text: 'name', value: 'id' };
  public selectedSkills: number[] = [1, 3]; // pre-select Angular & TypeScript

  onChange(args: any): void {
    console.log('Selected IDs:', args.value);
  }
}
```

```css
/* styles.css */
@import '@syncfusion/ej2-base/styles/material3.css';
@import '@syncfusion/ej2-buttons/styles/material3.css';
@import '@syncfusion/ej2-inputs/styles/material3.css';
@import '@syncfusion/ej2-lists/styles/material3.css';
@import '@syncfusion/ej2-popups/styles/material3.css';
@import '@syncfusion/ej2-dropdowns/styles/material3.css';
@import '@syncfusion/ej2-angular-dropdowns/styles/material3.css';
```

## Common Patterns

### Checkbox Mode with Select All
```typescript
import { MultiSelectModule, CheckBoxSelectionService } from '@syncfusion/ej2-angular-dropdowns';

@Component({
  standalone: true,
  imports: [MultiSelectModule],
  providers: [CheckBoxSelectionService],
  template: `
    <ejs-multiselect
      [dataSource]="items"
      mode="CheckBox"
      [showSelectAll]="true"
      selectAllText="Select All"
      unSelectAllText="Unselect All"
      [maximumSelectionLength]="5">
    </ejs-multiselect>
  `
})
```

### Filtering with Remote Data
```typescript
template: `
  <ejs-multiselect
    [dataSource]="data"
    [allowFiltering]="true"
    filterType="contains"
    (filtering)="onFilter($event)">
  </ejs-multiselect>
`

onFilter(args: FilteringEventArgs): void {
  const query = new Query().where('name', 'contains', args.text, true);
  args.updateData(this.data, query);
}
```

### Reactive Form Integration
```typescript
import { ReactiveFormsModule, FormControl } from '@angular/forms';

// In component:
public skillsControl = new FormControl([1, 3]); // pre-selected IDs

// In template:
// <ejs-multiselect [formControl]="skillsControl" [dataSource]="skills" [fields]="fields">
// </ejs-multiselect>
```

## Key Props

| Property | Type | Purpose |
|---|---|---|
| `dataSource` | `any[] \| DataManager` | Items to display |
| `fields` | `FieldSettingsModel` | Maps `text`, `value`, `groupBy`, `disabled`, `iconCss` |
| `value` | `any[]` | Currently selected values (use `[(value)]` for two-way) |
| `mode` | `string` | `Default`, `Box`, `Delimiter`, `CheckBox` |
| `allowFiltering` | `boolean` | Enables search within the dropdown |
| `filterType` | `string` | `startsWith`, `contains`, `endsWith` |
| `showSelectAll` | `boolean` | Shows Select All in CheckBox mode |
| `maximumSelectionLength` | `number` | Caps the number of selectable items |
| `allowCustomValue` | `boolean` | Lets users add values not in the list |
| `enableVirtualization` | `boolean` | Optimizes rendering for large lists |
| `popupHeight` / `popupWidth` | `string` | Constrains popup dimensions |
| `allowResize` | `boolean` | Lets users drag-resize the popup |
| `placeholder` | `string` | Input hint text |
