---
name: syncfusion-angular-drop-down-list
description: Implement the Syncfusion Angular DropDownList component for single-value selection from a predefined list. Use this when building dropdown selectors, searchable dropdowns, filtered lists, grouped options, or cascading dropdowns. This skill covers data binding, filtering, templates, grouping, virtualization, form integration, styling, and API usage with @syncfusion/ej2-angular-dropdowns.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
  category: "DropDowns"
---

# Syncfusion Angular DropDownList

The DropDownList component allows users to select a single value from a predefined list. It supports local and remote data sources, filtering, grouping, custom templates, virtualization for large datasets, and full Angular forms integration.

## Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installation and Angular CLI setup
- Installing `@syncfusion/ej2-angular-dropdowns`
- CSS/theme imports for Material3
- Adding `<ejs-dropdownlist>` to the template
- Basic data binding with `[dataSource]`
- Popup height/width configuration
- Two-way binding with `[(value)]`

### Data Binding
📄 **Read:** [references/data-binding.md](references/data-binding.md)
- Binding primitive arrays (strings, numbers)
- Binding arrays of objects with `[fields]` mapping
- Binding to nested/complex objects
- Remote data via `DataManager` (OData, Web API)
- Async pipe with Observable data streams
- Value binding (primitive and object types with `allowObjectBinding`)

### Filtering
📄 **Read:** [references/filtering.md](references/filtering.md)
- Enabling search with `[allowFiltering]`
- Using the `filtering` event and `updateData()` method
- Minimum character threshold before filtering starts
- Filter types: `contains`, `startsWith`, `endsWith`
- Case-sensitive filtering
- Diacritics/accent-insensitive filtering
- Debounce delay for performance optimization

### Templates
📄 **Read:** [references/templates.md](references/templates.md)
- Item template: customize each list item
- Value template: customize the selected value display
- Group template: customize group header appearance
- Header template: static element at popup top
- Footer template: static element at popup bottom
- No-records template: empty state display
- Action failure template: error state display

### Grouping & Virtualization
📄 **Read:** [references/grouping-and-virtualization.md](references/grouping-and-virtualization.md)
- Grouping items with `groupBy` field
- Inline and fixed floating group headers
- Virtual scrolling with `[enableVirtualization]` for large lists
- Virtual scrolling with remote data
- Combining filtering and virtualization
- Customizing item count in virtual mode

### Disabled Items & Forms
📄 **Read:** [references/disabled-items-and-forms.md](references/disabled-items-and-forms.md)
- Disabling specific items via `fields.disabled`
- Dynamic `disableItem()` method (by value, index, or element)
- Disabling the entire component with `[enabled]="false"`
- Template-driven forms with `[(ngModel)]`
- Reactive forms with `FormControl` and `FormGroup`

### Customization & Styling
📄 **Read:** [references/customization-and-styling.md](references/customization-and-styling.md)
- CSS overrides for wrapper, icon, focus states
- Outline theme focus customization
- Popup appearance and list item styles
- Float label and placeholder styling
- Mandatory asterisk pattern
- Localization with `L10n` class
- RTL (right-to-left) support

### How-To Recipes
📄 **Read:** [references/how-to.md](references/how-to.md)
- Add / remove / clear items dynamically
- Close popup programmatically
- Cascading DropDownLists
- Customize group header template
- Highlight filtered characters
- Incremental search behavior
- Modify remote data results
- Remote data item count display
- Tooltip on list items
- Value change event handling
- Icon support in list items

## Quick Start Example

```typescript
import { Component, OnInit } from '@angular/core';
import { DropDownListModule } from '@syncfusion/ej2-angular-dropdowns';

@Component({
  standalone: true,
  imports: [DropDownListModule],
  selector: 'app-root',
  template: `
    <ejs-dropdownlist
      id="ddl"
      [dataSource]="sports"
      [fields]="fields"
      placeholder="Select a sport"
      [(value)]="selectedValue"
      (change)="onChange($event)">
    </ejs-dropdownlist>
    <p>Selected: {{ selectedValue }}</p>
  `
})
export class AppComponent implements OnInit {
  public sports: { id: number; name: string }[] = [];
  public fields = { text: 'name', value: 'id' };
  public selectedValue: number = 2;

  ngOnInit() {
    this.sports = [
      { id: 1, name: 'Badminton' },
      { id: 2, name: 'Cricket' },
      { id: 3, name: 'Football' },
      { id: 4, name: 'Tennis' }
    ];
  }

  onChange(args: any) {
    console.log('Selected value:', args.value);
  }
}
```

## Common Patterns

### Searchable Dropdown
```typescript
// Enable filtering in template
// <ejs-dropdownlist [allowFiltering]="true" (filtering)="onFilter($event)">
import { FilteringEventArgs } from '@syncfusion/ej2-angular-dropdowns';
import { Query } from '@syncfusion/ej2-data';

onFilter(args: FilteringEventArgs) {
  let query = new Query();
  query = args.text !== '' 
    ? query.where('name', 'contains', args.text, true) 
    : query;
  args.updateData(this.sports, query);
}
```

### Remote Data Dropdown
```typescript
import { DataManager, ODataV4Adaptor, Query } from '@syncfusion/ej2-data';

public remoteData: DataManager = new DataManager({
  url: 'https://services.odata.org/V4/Northwind/Northwind.svc/Customers',
  adaptor: new ODataV4Adaptor(),
  crossDomain: true
});
public remoteQuery = new Query().select(['CustomerID', 'ContactName']).take(6);
public remoteFields = { text: 'ContactName', value: 'CustomerID' };
// Template: <ejs-dropdownlist [dataSource]="remoteData" [query]="remoteQuery" [fields]="remoteFields">
```

### Reactive Form Integration
```typescript
import { FormBuilder, FormGroup, Validators, ReactiveFormsModule } from '@angular/forms';
import { DropDownListModule } from '@syncfusion/ej2-angular-dropdowns';

// In component:
form = this.fb.group({ sport: ['Cricket', Validators.required] });
// In template:
// <form [formGroup]="form">
//   <ejs-dropdownlist formControlName="sport" [dataSource]="sports"></ejs-dropdownlist>
// </form>
```

## Key Props

| Property | Type | Description |
|---|---|---|
| `dataSource` | `any[] \| DataManager` | Data for the list |
| `fields` | `FieldSettingsModel` | Maps text, value, groupBy, disabled, iconCss |
| `value` | `string \| number \| boolean \| object` | Selected value (supports two-way binding) |
| `placeholder` | `string` | Input placeholder text |
| `allowFiltering` | `boolean` | Enable search box in popup |
| `filterType` | `'contains' \| 'startsWith' \| 'endsWith'` | Filter match strategy |
| `enableVirtualization` | `boolean` | Virtual scrolling for large lists |
| `popupHeight` / `popupWidth` | `string \| number` | Popup dimensions |
| `enabled` | `boolean` | Enable/disable entire component |
| `allowObjectBinding` | `boolean` | Bind full object as value |
| `ignoreCase` | `boolean` | Case-insensitive filtering (default: true) |
| `ignoreAccent` | `boolean` | Diacritics-insensitive filtering |
| `debounceDelay` | `number` | Delay (ms) before filter triggers |
| `itemTemplate` | `string` | Template for list items |
| `valueTemplate` | `string` | Template for selected value display |
| `noRecordsTemplate` | `string` | Empty state message |

## Common Use Cases

**Form field with validation** → See [references/disabled-items-and-forms.md](references/disabled-items-and-forms.md)

**Large list (1000+ items)** → Enable `[enableVirtualization]="true"` — see [references/grouping-and-virtualization.md](references/grouping-and-virtualization.md)

**Country/State/City cascading** → See [references/how-to.md](references/how-to.md#cascading-dropdownlists)

**Custom item rendering** (icons, multi-column) → See [references/templates.md](references/templates.md)

**Remote API data** → See [references/data-binding.md](references/data-binding.md#binding-remote-data)
